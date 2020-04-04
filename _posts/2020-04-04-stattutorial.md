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

Output:
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
