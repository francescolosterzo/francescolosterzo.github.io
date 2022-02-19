---
layout: post
title: Custom rolling weighted average in pandas
date: 2022-02-19
---

I recently realized that while `pandas` provides a nice way to perform rolling operations (e.g. compute a rolling average), it does not provide any out of the box solution to implement even slightly more spicy things like a rolling weighted average, even more so if you want to take care of some exceptions. I ended up writing my own function, and this post contains a short explanation of how it works.

Here is what I want to do: compute the running average of some data, allowing the computation to happen even when there is less data available (e.g. for the very first points or when some values are missing). I also want to include weights in this calculation.

## How to compute a rolling average in pandas
Let’s start with what you can do with `pandas` by checking how [`pandas.DataFrame.rolling()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.rolling.html) method works on a set of dummy data (with a missing value):
```python
data = pd.Series( range(11) )
data.loc[6] = np.nan
data
```
```
0      0.0
1      1.0
2      2.0
3      3.0
4      4.0
5      5.0
6      NaN
7      7.0
8      8.0
9      9.0
10    10.0
dtype: float64
```
Let’s now compute the “3-point” running average, but setting `min_periods` to 2, i.e. the mean is computed even in cases in which only 2 points are available in the window:
```python
data.rolling(3, min_periods=2).mean()
```
```
      NaN
1      0.5
2      1.0
3      2.0
4      3.0
5      4.0
6      4.5
7      6.0
8      7.5
9      8.0
10     9.0
dtype: float64
```
In the above result, the first number is `NaN` since at that stage only the first value is in the window (and we set `min_periods` to 2), the second number is 0.5 because it is the mean of 0 and 1, and from the third row on we indeed obtain the average of the original value in the same row combined with the values on the two preceding rows, e.g. for row 5 we obtain `np.mean([5,4,3])` which is 4. Because of `min_periods=2`, the `NaN` in row 6 of the original data is ignored when computing the averages, e.g. the average on row 7 is `np.mean([7,5])`. Nothing new up to here.

## Can’t use weights right away
Let’s now try to introduce weights. For the sake of simplicity, let’s use equal weights so that the result will be the same as the above even if we are using a weighted mean.
```python
weights = [1,1,1]
data.rolling(3, min_periods=2).apply( lambda x: np.average(x, weights=weights) )
```
```
TypeError: Axis must be specified when shapes of a and weights differ.
```
Aha! Here comes an error complaining about different shapes! If you follow the suggestion of specifying the axis in `np.average` you get this error instead:
```
ValueError: Length of weights not compatible with specified axis.
```
The error comes indeed from our requirement of `min_periods=2`, which then introduces a mismatch between length the data being considered in the window and the length of the array of weights. If `min_periods` is set to the same length of the window, the above code works and the problem is solved, but the missing value in row 6 results in the three averages in which it is taken into account to be `NaN`s, i.e. the result is different from what we want.

## Here’s a solution
To solve this problem and then get an equivalent result as the simple rolling average I wrote this function:
```python
def weighted_mean(x, w=None):
    """
    compute the weighted mean.
    Input parameters:
    - x: data to average
    - w: weights, optional
    Return: the weighted mean of the data x with weights w
    """
    if w is None:
            w = np.repeat(1, len(x))
    
    w_to_use = np.array(w[-len(x):]).astype(float)
    nan_idx = np.where(np.isnan(x))[0]
    w_to_use[nan_idx] = np.nan
    
    return np.nansum(x * w_to_use) / np.nansum(w_to_use)
```
Here is the result of applying the function to our problem:
```python
data.rolling(3, min_periods=2).apply( lambda x: weighted_mean(x, w=weights) )
```
```
0      NaN
1      0.5
2      1.0
3      2.0
4      3.0
5      4.0
6      4.5
7      6.0
8      7.5
9      8.0
10     9.0
dtype: float64
```
We got exactly the same result as with the plain running average, but we’re now allowed to compute a real weighted running average in the presence of missing data!
 
Let’s now dive through the details of the function. There are two key parts which contribute to solve the problem:
* on the 7th row the length of the vector of weights is adapted to the length of the data: this takes care of the mismatch between data and weights, preventing python from complaining about “Length of weights not compatible with specified axis”
* on the 8th and 9th rows, the weights corresponding to missing values in the data are set to `NaN`. This part simply adapts the denominator of the average to the number of points that are indeed available: this is what allows the average in row 6 to be 4.5, i.e. (4+5)/2 instead of 3, i.e. (4+5)/3

In the above function the weights are handled in the same order as in `np.average()`, i.e. the vector of weights undergoes an element-wise multiplication with the elements of the data array included in the window. Therefore setting `weights = [0,0,1]` returns exactly the same starting data (with `NaN` in the first row due to `min_periods`):
```python
weights = [0,0,1]
data.rolling(3, min_periods=2).apply( lambda x: weighted_mean(x, w=weights) )
```
```
0      NaN
1      1.0
2      2.0
3      3.0
4      4.0
5      5.0
6      NaN
7      7.0
8      8.0
9      9.0
10    10.0
dtype: float64
```

## Conclusions
The `weighted_mean` function allows to compute the weighted running average of some data, overcoming the constraint of having `min_periods` equals to size the window and handling missing data without affecting the results.

The whole code undelying this post is in a [dedicated notebook](). Feel free to take a look at it, download it and tailor it to your needs.

If you find a mistake or want to tell me something else get in touch on twitter [@francescolost](https://twitter.com/flosterzo)
