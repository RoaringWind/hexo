---
title: Golang slice
date: 2021-08-03 19:45:03
tags: [Golang,Go]
---
```go
x := make([]int, 3, 3)
x[0]=1;x[1]=2;x[2]=3
y:=x[1:];y[0]=99
fmt.Println(x,y) // [1,99,3] [99,3]
fmt.Println(len(y),cap(y)) // 2 2 
fmt.Println(len(x),cap(x)) // 3 3
y = append(y, 1) // 容量不够，所以分配了一块新内存，把原有的值挪上去
y[0] = -1
fmt.Println(x,y) // [1 99 3] [-1 3 1]

var slice1 []int
slice2:=make([]int,0)
slice3:= []int{} // 分配了内存
fmt.Println(slice1,slice2,slice3) // [] [] []
fmt.Println(len(slice1),cap(slice1)) // 0 0
fmt.Println(len(slice2),cap(slice2)) // 0 0
fmt.Println(len(slice3),cap(slice3)) // 0 0
fmt.Println(reflect.DeepEqual(slice1,slice2)) // false
fmt.Println(reflect.DeepEqual(slice1,slice3)) // false
fmt.Println(reflect.DeepEqual(slice2,slice3)) // true
```
