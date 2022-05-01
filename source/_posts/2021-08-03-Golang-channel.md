---
title: Golang channel
date: 2021-08-03 20:24:28
tags: [Golang,Go,信号]
---
Go的无缓冲`channel`的发送/接收操作会阻塞直到被接收/发送。如果是在goroutinue里面，怎么阻塞都不要紧，以为main结束了goroutine也就结束了。但如果在main里面阻塞就会报panic(fatal error: all goroutines are asleep - dead lock!)
```go

package main

import (
	"fmt"
	"sync"
)

func main() {
	wg := sync.WaitGroup{}
	times := 10
	wg.Add(times)
	for i := 0; i < times; i++ {
		go func() {
			fmt.Println(i) // i指向循环段的变量，需要用临时变量代替才不会出错，这样写会有问题，打印出来的大部分数字都是times，而且是随机的。
			wg.Done()
		}()
	}
	wg.Wait()
	fmt.Println("all done")
}
```
channel和WaitGroup都可以用来完成并发的工作，channel更注重于通信，wg不通信。
```go
sig := make(chan os.Signal)
signal.Notify(sig,syscall.SIGINT,syscall.SIGTERM) // 可以用来作等待，等待ctrl-c终止信号，不加后面的值就监测所有信号。
```
```go
chan T          // 可以接收和发送类型为 T 的数据
chan<- float64  // 只可以用来发送 float64 类型的数据
<-chan int      // 只可以用来接收 int 类型的数据
```
