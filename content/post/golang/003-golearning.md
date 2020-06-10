---
title: "003 golang快速入门(3) 流程控制"
date: 2020-06-05T23:04:06+08:00
draft: false
tags: [golang]
---

go语言作为类C语言的一种，流程控制与其他编程语言没有什么不同，主要有以下几种控制结构

- 选择结构`if/Else`
- 多分支选择结构`switch`
- 循转结构`for`

#### 二、选择结构

条件语句是指定一个或多个条件，并通过测试条件是否为 true 来决定是否执行指定语句，并在条件为 false 的情况在执行另外的语句。

```go
package main

import (
	"fmt"
)

func main() {
	// 1、简单选择结构
	if 5%2 == 0 {
		fmt.Println("偶数")
	} else {
		fmt.Println("奇数")
	}

	// 2、可以不使用else
	if 8%4 == 0 {
		fmt.Println("8 可以被 4 整除")
	}

	// 3、可以在语句中定义
	if num := 9; num < 0 {
		fmt.Println(num, "是一个负数")
	} else if num < 10 {
		fmt.Println(num, "是个位数")
	} else {
		fmt.Println(num, "10位数以上的数组")
	}
}
```

输出结果

```
奇数
8 可以被 4 整除
9 是个位数
```

#### 三、多分支选择结构

go语言中的switch语句更加灵活多变。go语言的语句还可以做进行类型判断

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	i := 2
	fmt.Print("Write ", i, " as \n")
	// 1、go的switch 没有break
	switch i {
	case 1:
		fmt.Println("one")
	case 2:
		// 进入这个分支
		fmt.Println("two")
	case 3:
		fmt.Println("three")
	}

	// 2、可以在同一个case语句中用逗号分割，表示多个条件
	switch time.Now().Weekday() {
	case time.Saturday, time.Sunday:
		fmt.Println("It's the weekend")
	default:
		fmt.Println("It's a weekday")
	}

	// 3、default 默认分支与if/else类似
	t := time.Now()
	switch {
	case t.Hour() < 12:
		fmt.Println("It's before noon")
	default:
		fmt.Println("It's after noon")
	}

	// 4、实现类型检测
	whatAmI := func(i interface{}) {
		switch t := i.(type) {
		case bool:
			fmt.Println("I'm a bool")
		case int:
			fmt.Println("I'm an int")
		default:
			fmt.Printf("Don't know type %T\n", t)
		}
	}

	whatAmI(true)
	whatAmI(1)
	whatAmI("hey")
}
```

输出结果

```
Write 2 as 
two
It's the weekend
It's after noon
I'm a bool
I'm an int
Don't know type string
```



#### 四、循环结构

go只有一种循环结构`for`:

```go
package main

import (
	"fmt"
)

func main() {
	// 1、基本类型的循环结构
	i := 1
	for i <= 3 {
		fmt.Println("i = ", i)
		i = i + 1
	}

	// 2. 常见的循环结构，带条件和赋值
	for j := 10; j >= 5; j-- {
		fmt.Println("j = ", j)
	}

	// 3. 不带任何条件的循环语句，类似while
	loop := 1
 	// 这是一个死循环的句型
	for {
		if loop > 2 {
      // break 终止当前循环
			break
		}
		fmt.Println("loop", loop)
		loop = loop + 1
	}
  
 	// 4. 跳过本次循环
	for n := 0; n <= 5; n++ {
		if n%2 == 0 {
			fmt.Println("continue n=", n)
      // continue 跳过本次循环
			continue
		}
		fmt.Println("n=", n)
	}
}
```

输出结果

```
i =  1
i =  2
i =  3
j =  6
j =  5
loop 1
loop 2
continue n= 0
n= 1
continue n= 2
n= 3
continue n= 4
n= 5
```




