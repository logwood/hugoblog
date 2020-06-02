---
title: "golang快速入门前言"
date: 2020-06-02T22:54:32+08:00
draft: false
tags: [golang]
---

## 1、教程简介

本教程目的：与看到本教程的人一起，学习golang这门语言，分享学习成果，和热爱编程的同志们一起成长。

>Go是一种开放源代码编程语言，旨在构建简单，快速且可靠的软件。详细说明文章请参见[golang官网](https://golang.org/)

## 2、Hello World

废话不多说，我们直接上代码，Everything is Hello World

2.1 输入以下内容到hello-world.go
```
package main

import "fmt"

func main() {
    fmt.Println("Hello World!")
}
```
这就是一个完整的go的程序了

2.2 在安装了golang环境的的命令行，执行以下命令
```
# 直接运行程序
$ go run hello-world.go 
Hello World

# 编译程序
$ go build hello-world.go 

# 执行程序
$ ./hello-world 
Hello World
```


## 3、恭喜你

如果你的输出结果和我一样，恭喜你，你已经学会了，和我达到一样的水平了。


