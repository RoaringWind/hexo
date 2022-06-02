### slice是视图，包括了ptr,len,cap三个参数

只支持封装，不支持继承和多态。没有class只有struct。

go都是值传递

无论变量还是函数，首字母大写作为Public，小写private。

每个目录一个包，包名字可以和目录名字不一样。

目录下只能有一个main包

为结构定义的方法必须在同一个包里，但是可以是不同文件。



如何扩充系统类型或者别人的类型

- 定义别名
- 使用组合

> go get github.com/gpmgo/gopm
>
> gopm get -g -v -u golang.org/x/tools/cmd/goimports  #只是下载，没有build
>
> go install golang.org/x/tools/cmd/goimports #安装到bin
>
> 
>

Goland 里GO Modules设置GOPROXY=[https://goproxy.cn,direct](https://goproxy.cn%2Cdirect/)

go build #编译
go install #产生pkg和可执行文件
go run #编译运行

src下面放着项目
pkg下面是build出来的中间过程
bin下面是可执行文件