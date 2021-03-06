---
title: "Statistical Tools"
date: 2020-04-04
tags: [stat, python]
header:
  image: "/images/statheader.png"
excerpt: "Data Science"
mathjax: "true"
---

## ECDF
Python code block for ECDF:
```python
    import numpy as np

    def ecdf(df,col_name):  
      # col_name (str): name of the column in dataframe df 
      x = np.sort(df[col_name])
      y = np.arange(1, len(x)+1) / len(x)
      return x,y
```
Computing percentiles `np.percentile(df['col_name'], [25, 50, 75])`.

<img src="{{ site.url }}{{ site.baseurl }}/images/ecdf.png" alt="ecdf plot with percentiles">

## Poisson Process
* The timing of the next event is completely independent of when the previous event happened
* Examples of Poisson processes:
  1. Natural births in a given hospital
  2. Hit on a website during a given hour
  3. Meteor strikes
  4. Molecular collisions in a gas
  5. Aviation incidents
  
Mathematically: The number $$r$$ of arrivals of a Poisson process in a given time interval with average rate of $$λ$$ arrivals
per interval is Poisson distributed.
Example: The number $$r$$ of hits on a website in one hour with an average hit rate of 6 hits per hour is Poisson distributed.
Poisson distributed: `np.random.poisson(6, size=10000) `

Output:
<img src="{{ site.url }}{{ site.baseurl }}/images/posision.png" alt="ecdf plot of poisson distribution">

## Bootstrap confidence interval
* In statistical inference, you want to know what would happen if you could repeat your data acquisition an infinite number of times. The technique to do it is aptly called bootstrapping.
* Python code block for Bootstrapping:
```python
      def bootstrap_replicate_1d(data, func):
        """Generate bootstrap replicate of 1D data."""
        # Bootstrap sample(bs_sample ): a resampled array of the data
        bs_sample = np.random.choice(data, len(data)) 
        # Bootstrap sample(func(bs_sample) ): statistic computed from a resampled array
        return func(bs_sample)
```
```python  
    def draw_bs_reps(data, func, size=1):
        """ This function Generates many bootstrap replicates """

        # Initialize array of replicates: bs_replicates
        bs_replicates = np.empty(size)

        # Generate replicates:Draw bootstrap replicates.
        for i in range(size):
            bs_replicates[i] = bootstrap_replicate_1d(data, func)
        return bs_replicates  
```

+ Confidence interval of a statistic: If we repeated measurements over and over again, $$p%$$ of the observed values would lie within the $$p%$$ confidence interval.
- Example:
   1. Take 10,000 bootstrap replicates of the mean:  `bs_replicates = draw_bs_reps(data,np.mean,10000)`.
   2. Bootstrap confidence interval: `conf_int = np.percentile(bs_replicates, [2.5, 97.5])`.
   
<img src="{{ site.url }}{{ site.baseurl }}/images/bootstrap.png"  height="10"  alt="Histogram showing bootstrap replicates in 95 confidence interval">

### Pairs bootstrap:
* For Linear regression:
  1. Resample data in pairs
  2. Compute slope and intercept from resampled data
  3. Each slope and intercept is a bootstrap replicate
  4. Compute confidence intervals from percentiles of bootstrap
 
 Python code block for Pairs bootstrap in linear regression:
```python
    def draw_bs_pairs_linreg(x, y, size=1):
    """Perform pairs bootstrap for linear regression."""

    # Set up array of indices to sample from: inds
    inds = np.arange(len(x))

    # Initialize replicates: bs_slope_reps, bs_intercept_reps
    bs_slope_reps = np.empty(size)
    bs_intercept_reps = np.empty(size)

    # Generate replicates
    for i in range(size):
        bs_inds = np.random.choice(inds, size=len(inds))
        bs_x, bs_y = x[bs_inds], y[bs_inds]
        bs_slope_reps[i], bs_intercept_reps[i] = np.polyfit(bs_x, bs_y,1)

    return bs_slope_reps, bs_intercept_reps
```
Output:
<img src="{{ site.url }}{{ site.baseurl }}/images/pairbootstrap.svg" height="10" alt="ecdf plot of pair bootsrap for linear regression">

## hypothesis testing
   * Hypothesis testing: Assessment of how reasonable the observed data are assuming a hypothesis is true
   * Null hypothesis: name for the hypothesis you are testing
   * permutation replicates:  a single value of a statistic computed from a permutation sample.
   
```python
   def permutation_sample(data1, data2):
       """Generate a permutation sample from two data sets."""

       # Concatenate the data sets: data
       data = np.concatenate((data1,data2))

       # Permute the concatenated array: permuted_data
       permuted_data = np.random.permutation(data)

       # Split the permuted array into two: perm_sample_1, perm_sample_2
       perm_sample_1 = permuted_data[:len(data1)]
       perm_sample_2 = permuted_data[len(data1):]
       
       return perm_sample_1, perm_sample_2
```
```python
   def draw_perm_reps(data_1, data_2, func, size=1):
       """Generate multiple permutation replicates."""

       # Initialize array of replicates: perm_replicates
       perm_replicates = np.empty(size)

       for i in range(size):
           # Generate permutation sample
           perm_sample_1, perm_sample_2 = permutation_sample(data_1, data_2)

           # Compute the test statistic
           perm_replicates[i] = func( perm_sample_1, perm_sample_2)
       return perm_replicates
```
   * p-value: The probability of obtaining a value of your test statistic that is at least as extreme as what was observed, under the assumption the null hypothesis is true.
   * Statistical significance is determined by the smallness if a p-value
### Bootstrap hypothesis testing pipeline
    1. Clearly state the null hypothesis 
    2. Define your test statistic
    3. Generate many sets of simulated data assuming the null hypothesis is true
    4. Compute the test statistic for each simulated data set
    5. The p-value is the fraction of your simulated data sets for which the test statistic is at least as extreme as for the real data
    
    
    
    
    
    
    
    
    
