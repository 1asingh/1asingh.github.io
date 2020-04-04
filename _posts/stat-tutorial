---
title: "stat tutorial"
date: 2020-04-04
tags: [stat, python]
header:
  image: "/images/perceptron/percept.jpg"
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
