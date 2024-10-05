# Mean Reversion Analysis of Reliance Returns Using Time Series Data

This project aims to analyze whether the 1-minute time frame returns of Reliance stock are mean-reverting by checking if the data is stationary. Three tests are used to check the stationarity of the time series:


# Stationarity Tests

Used three different tests to analyze the stationarity of the time series:
- Augmented Dickey-Fuller (ADF) Test
- Kwiatkowski-Phillips-Schmidt-Shin (KPSS) Test
- Phillips-Perron (PP) Test
# Table of Contents

- [Introduction](#introduction)
- [Requirements](#requirements)
- [Data Preprocessing](#data-preprocessing)
- [Stationarity Tests](#stationarity-tests)
  - [Augmented Dickey-Fuller (ADF) Test](#augmented-dickey-fuller-adf-test)
  - [Kwiatkowski-Phillips-Schmidt-Shin (KPSS) Test](#kwiatkowski-phillips-schmidt-shin-kpss-test)
  - [Phillips-Perron (PP) Test](#phillips-perron-pp-test)
- [Interpreting Results](#interpreting-results)
- [Conclusion](#conclusion)

---

## Introduction
# Mean Reversion Analysis of Reliance Returns Using Time Series Data
##### This project aims to analyze whether the 1-minute time frame returns of Reliance stock are mean-reverting by checking if the data is stationary. Three tests are used to check the stationarity of the time series:
Augmented Dickey-Fuller (ADF) Test
Kwiatkowski-Phillips-Schmidt-Shin (KPSS) Test
Phillips-Perron (PP) Test

## Requirements
List of required libraries:
- matplotlib
- arch
- pandas 
- statsmodels

## Installation
```bash
pip install pandas matplotlib  statsmodels arch
```

## Imports

```python
import pandas as pd
import matplotlib.pyplot as plt
import warnings
from arch.unitroot import PhillipsPerron
from statsmodels.tsa.stattools import kpss
from statsmodels.tsa.stattools import adfuller
```

## Loading data

```python
df = pd.read_csv('reliance.csv')
df['return'] = df['RELIANCE-EQ'].pct_change()
df.dropna(inplace=True)
```
## Plotting data
```
# Plot the time series
plt.figure(figsize=(12, 6))
plt.plot(df['return'], label='Return')
plt.title('Time Series Plot of Reliance Return')
plt.xlabel('Date')
plt.ylabel('Return')
plt.legend()
plt.show()
```
## Result storing in dictionary
```python
test_ = {} #blank dict to store result
```
## Augmented Dickey-Fuller (ADF) Test (Test-1)
The ADF test checks for the presence of a unit root in the time series. The null hypothesis assumes there is a unit root (non-stationary data), and the alternative hypothesis assumes the data is stationary.


```python
from statsmodels.tsa.stattools import adfuller

def perform_adf_test(series, title=''):
    """
    Perform Augmented Dickey-Fuller test and print results.
    """
    print(f'Augmented Dickey-Fuller Test: {title}')
    result = adfuller(series, autolag='AIC')
    labels = ['ADF Statistic', 'p-value', '# Lags Used', '# Observations Used']
    for value, label in zip(result[:4], labels):
        print(f'{label}: {value}')
    for key, value in result[4].items():
        print(f'Critical Value ({key}): {value}')
    if result[1] < 0.05:
        print("=> Reject the null hypothesis. The series is stationary.")
    else:
        print("=> Fail to reject the null hypothesis. The series is non-stationary.")
    print('\n')

# ADF Test on return's time series
perform_adf_test(df['return'], 'Return  Series')
```
## ADF Test on  return series output
```
KPSS Test:
KPSS Statistic: 0.49288824861882913
p-value: 0.043268412473236685
Num Lags: 4
Critical Value (10%): 0.347
Critical Value (5%): 0.463
Critical Value (2.5%): 0.574
Critical Value (1%): 0.739
=> Reject the null hypothesis. The series is non-stationary.
```
## PhillipsPerron(PP) Test
The Phillips-Perron (PP) test is a non-parametric test that is an alternative to the Augmented Dickey-Fuller (ADF) test. It adjusts for serial correlation and heteroscedasticity using a kernel-based correction. A stationary series means that it has a constant mean, variance, and autocovariance.

The test null hypothesis (H₀) is that the series has a unit root (i.e., the time series is non-stationary). The alternative hypothesis (H₁) is that the series is stationary.

If the p-value is less than 0.05, we reject the null hypothesis and conclude that the series is stationary.
If the p-value is greater than 0.05, we fail to reject the null hypothesis and conclude that the series is non-stationary.

```python
from arch.unitroot import PhillipsPerron

def perform_pp_test(series, title=''):
    """
    Perform Phillips-Perron test and print results.
    """
    print(f'Phillips-Perron Test: {title}')
    result = PhillipsPerron(series, )
    p_value = result.pvalue
    if p_value < 0.05:
        test_.update({'PhillipsPerron':'Rejected Ho'})
        statement = 'Rejected Ho'
    else:
        statement = 'Fail to Rejected Ho'
        test_.update({'PhillipsPerron':'Fail to Rejected Ho'})
    print(result)
    print("*"*60)
    print(statement)
    labels = ['PP Statistic', 'p-value', '# Lags Used', '# Observations Used']
# Perform PP Test on return series
perform_pp_test(df['return'], 'Return Series')
```
## PP Test on return series Output
```console
Phillips-Perron Test:Return Series
     Phillips-Perron Test (Z-tau)    
=====================================
Test Statistic                -55.063
P-value                         0.000
Lags                               29
-------------------------------------

Trend: Constant
Critical Values: -3.43 (1%), -2.86 (5%), -2.57 (10%)
Null Hypothesis: The process contains a unit root.
Alternative Hypothesis: The process is weakly stationary.
************************************************************
Rejected Ho
```
## Printing Results 
```python
print(test_)
```
## Output
```python
{'adfuller': 'Rejected Ho',
 'kpss': 'Rejected Ho',
 'PhillipsPerron': 'Rejected Ho'}
```
# Conclusion
An all the three test have rejected the Ho means there is no unitroot problem which indirectly implies that the reliance returns are meanreverting.

