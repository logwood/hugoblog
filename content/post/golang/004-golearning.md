---
title: "004 golang快速入门(4) Array & Slice"
date: 2020-06-09T23:14:36+08:00
draft: true
tags: [golang]
---

本节主要介绍数组(array)与切片(slice)，也比较让人困惑。go语言数组与切片都属于集合类的类型。但数组类型的声明长度必须给定，而且之后不会再改变。切片的长度可以自动地随着其中元素数量的增长而增长，但不会随着元素数量的减少而减小。简而言之：**数组是固定长度，切片是可变长度。** go语言中切片可以看成数组的一个特例，这里表达不一定准备，大家自行体会吧。

- Array的使用
- Slice的使用

#### 一、Array的使用

##### 1.1 数组定义

```go
// 定一个长度位5的数组
	var arr [5]int
	// 初始化输出的数组会填充为0 [0 0 0 0 0]
	fmt.Println("arr:", arr)

	// 给下标为4的数组赋值
	arr[4] = 100
	// 输出：set: [0 0 0 0 100]
	fmt.Println("set:", arr)
	// get: 100
	fmt.Println("get:", arr[4])
	// len获取数组长度 len: 5
	fmt.Println("len:", len(arr))

	// 带有值的数组定义
	b := [5]int{1, 2, 3, 4, 5}
	// dcl: [1 2 3 4 5]
	fmt.Println("dcl:", b)

	// 二维数组定义
	var twoD [2][3]int
	for i := 0; i < 2; i++ {
		for j := 0; j < 3; j++ {
			twoD[i][j] = i + j
		}
	}
	// 2d:  [[0 1 2] [1 2 3]]
	fmt.Println("2d: ", twoD)

 
```

##### 1.2 数组遍历

```go
b := [5]int{1, 2, 3, 4, 5}
// 普通的方式遍历
for i := 0; i < len(b); i++ {
  fmt.Println("index:", i, "value:", b[i])
}
// 常见遍历方法
for i, v := range b {
  fmt.Println("index:", i, "value:", v)
}
```


#### 二、Slice的使用

