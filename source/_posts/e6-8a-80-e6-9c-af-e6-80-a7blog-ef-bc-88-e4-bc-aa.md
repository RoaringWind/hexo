---
title: 技术性Blog（伪
url: 226.html
id: 226
categories:
  - Machine Learning
date: 2020-01-12 23:13:40
tags: [Python]
---

荐书《Python深度学习》，人民邮电出版社，张亮译。此书非常易懂！！ Environment: Python 3.7.4 Backend: TensorFlow Python技巧: 

```python
import numpy as np re=np.zeros((8,16))#8行16列的矩阵，所有元素为0
re[[1,2],[4,8]]=4#可行，前后数量相等，等价于re[1,4]=re[2,8]=5
re[[2],[4,8]]=5#可行，有一个维度数量为1，怎么都可行，[x]和x等价
re[:,]=5#可行，全部置为5 re[,]=5#不可行
re[1:3,[4,6,7,8]]=5 #可行，第1，2行的4678列置为5
a=list([3,2,1])
for index,sequence in enumerate(a):#枚举下标和元素值
    print(index,sequence)
stu={‘wang’:’ba’}#等价于stu=dict([(‘wang’,’ ba’)])
stu[‘wang’]#输出’ba’
reverse_index=dict([(value,key)for (key,value) in stu.items()])
range(1,3)#取到1，2，不取3
A = numpy.matrix(numpy.ones((3,3)))
numpy.array(A)[2]=2 #不改变A
numpy.asarray(A)[2]=2 #改变A 
```

Keras工作流程： 

(1) 定义训练数据：输入张量和目标张量。
(2) 定义层组成的网络（或模型），将输入映射到目标。 
(3) 配置学习过程：选择损失函数、优化器和需要监控的指标。
(4) 调用模型的 fit 方法在训练数据上进行迭代。 定义模型有两种方法：一种是使用 Sequential 类（仅用于层的线性堆叠），另一种是函数式 API（ 用于层组成的有向无环图，可以构建任意形式的架构）。

写到一半发现还是暂时鸽了吧2333，后面画一个思维导图会更好讲明白一点。