---
title: "008 Dependency Inversion Principle"
date: 2020-08-13T22:29:14+08:00
draft: false
tags: [principles]
---

###  依赖倒置原则

依赖倒置原则(DIP)由Robert C. Martin在1996年在 [Dependency Inversion Principle](http://butunclebob.com/ArticleS.UncleBob.PrinciplesOfOod)中提出，他对此定义如下:

> *High level modules should not depend on low level modules. Both should depend on abstractions. Abstractions should not depend upon details. Details should depend on abstractions*
> *Robert C. Martin*
>
> *顶层模块不应该依赖底层模块。这2者都应该依赖于抽象。抽象不应该依赖于细节。细节应该依赖于抽象。*

程序要依赖于抽象接口，不要依赖于具体实现。简单的说就是要求对抽象进行编程，不要对实现进行编程。也被称作好莱坞原则：Don’t call me，I will call you.其核心思想是:**要面向接口编程，不要面向实现编程。**

#### 下面我看一个实例

Example:张三开车的例子，张三是一名司机，可以开不不同品牌的汽车。

```go
// 面向对象设计原则：DIP依赖倒置原则
// 司机可开任何汽车——依赖抽象/接口

// 汽车 Interface
type ICar interface {
	run()
}

type Camary struct{}

func (C Camary) run() {
	fmt.Println("Camary runing")
}

type Benz struct{}

func (C Benz) run() {
	fmt.Println("Benz runing")
}

// 司机 Interface
type IDriver interface {
	// 司机会驾驶的汽车
	Ddrive(car ICar)
}

type Driver struct {
	Name string
}

func (D *Driver) Ddrive(car ICar) {
	fmt.Println(D.Name)
	car.run()
}

```

调用

```go
func TestDriver(t *testing.T) {
	zhangsan := principles.Driver{Name: "zhangsan"}
	car := principles.Camary{}
	zhangsan.Ddrive(car)
	car1 := principles.Benz{}
	zhangsan.Ddrive(car1)
}

---
=== RUN   TestDriver
zhangsan
Camary runing
zhangsan
Benz runing
--- PASS: TestDriver (0.00s)
```

看看以上代码。任意司机可以看任意品牌的汽车，相互之间没有依赖。各汽车之间也可进行定制开发，增加不同的功能。

满足依赖倒置的原则：

- 高层模块不应该依赖低层模块，都应该依赖抽象(接口或抽象类)
- 接口或抽象类不应该依赖于实现类
- 实现类应该依赖于接口或抽象类

发现了没，是不感觉这些实现，代码重复了。

