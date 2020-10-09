---
title: Linear Programming
date: 2020-08-18 17:17:54
tags: 
---

```python
import numpy as np
from scipy.optimize import linprog
a = np.array([1, 1, 1])
b = np.array([-7.9])
c = np.array([[-3.05, -4.05, -6.1]])
res = linprog(a, A_ub=c, b_ub=b)
print(res)
```

官方参考:
https://docs.scipy.org/doc/scipy/reference/generated/scipy.optimize.linprog.html