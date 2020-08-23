---
title: PCA analysis
date: 2020-08-18 15:18:05
tags: [ML,Python]
---

#  作用

用于数据降维，举例来说，太阳实际是*球体*，但一般可以说太阳是*圆的* 。这就是数据降维。



# 用法

```Python
import numpy as np
from sklearn.decomposition import PCA
X = np.array([[-1, -1], [-2, -1], [-3, -2], [1, 1], [2, 1], [3, 2]])
pca = PCA(n_components=2) # 数据维数，设置为'mle'是自动保留
pca.fit(X)
print(pca.explained_variance_ratio_)
>>>[ 0.99244...  0.00755...]
print(pca.transform(X)) # 做一个每一列到其他列的投影
>>>[[ 1.38340578]
 [ 2.22189802]
 [ 3.6053038 ]
 [-1.38340578]
 [-2.22189802]
 [-3.6053038 ]]
```

