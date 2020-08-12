---
title: "007 Single Responsibility Principle"
date: 2020-07-31T22:51:03+08:00
draft: false
tags: [principles]
---

## 单一职责原则

> 软件设计的两个基本准则：低耦合和高内聚

### 什么是单一职责原则

**单一职责原则，指的是一个类或者模块有只有一 个改变的原因。**

- 单一职责可以降低类的复杂性，提高代码可读性、可维护性



我们举例说明单一职责， 看下面一段代码,我们实现一个开关，开灯关灯的实例 

```go

const (
	// 打开状态
	STATE_OK int = 1
	// 关闭状态
	STATE_ON int = 0
)

// 开关
type Button interface {
	// 获取状态
	GetState() string
	// 设置状态
	SetState(int)
}

// 灯泡
type Lamp struct {
	State string
}

// 获取灯泡状态
func (b *Lamp) GetState() string {
	return b.State
}

// 设置灯泡状态
func (b *Lamp) SetState(state int) {
	switch state {
	case STATE_OK:
		b.State = "开灯"
	case STATE_ON:
		b.State = "关灯"
	default:
		b.State = "未知状态"
	}
}
```

调用代码

```go
	var btn principles.Button
	lamp := principles.Lamp{State: "关灯"}
	// 实现组合
	btn = &lamp

	// 开灯
	btn.SetState(1)
	t.Log("灯泡状态：", btn.GetState())

	// 关灯
	btn.SetState(0)
	t.Log("灯泡状态：", btn.GetState())
----------
=== RUN   TestButton_SetName
--- PASS: TestButton_SetName (0.00s)
    principles_test.go:122: 灯泡状态： 开灯
    principles_test.go:126: 灯泡状态： 关灯
```

在SetState这种实现在我们代码中就经常看见，由switch..case 的当时进行状态或逻辑处理。这里可以看出，我们这个函数是担任多个职责的，如果再新增一个状态，就要修个这个代码。如果是在复杂的代码中会变的难以维护。

下面我们简单修改下，就行代码职责的修改

```go
// 开关
type Button interface {
	// 获取状态
	GetState() string
	// 开灯
	OpenState()
	// 关
	CloseState()
}

func (b *Lamp) OpenState() {
	b.State = "开灯"
}

func (b *Lamp) CloseState() {
	b.State = "开灯"
}
```

调用代码

```go
// 开灯
	btn.OpenState()
	t.Log("灯泡状态：", btn.GetState())

	// 关灯
	btn.CloseState()
	t.Log("灯泡状态：", btn.GetState())
```

看到这样代码，是不是感觉清晰了，如果在开灯的地方需要加一个延时打开灯泡的代码，是否不用关心关闭的代码。



这就是软件设计的单一职责原则。如果一个类或者函数承担的职责太多，就等于把这些职责都耦合在一起。这种耦合会导致类很脆弱：当变化发生的时候，会引起类不必要的修改，进而导致 bug 出现。

职责太多，还会导致类的代码太多。一个类太大，它就很难保证满足开闭原则，如果不得不打开类文件进行修改，大堆大堆的代码呈现在屏幕上，一不小心就会引出不必要的错误。

写出好的代码的最佳实践，首先是**控制一个文件的行数**