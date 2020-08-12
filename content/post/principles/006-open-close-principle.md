---
依赖倒置原则是这样的title: "006 Open Close Principle"
date: 2020-07-28T22:17:45+08:00
draft: false
tags: [principles]
---

## 开闭原则实践

开闭原则（Open Close Principle），开闭原则就是说对扩展开放，对修改关闭。在程序需要进行拓展的时候，不能去修改原有的代码，实现一个热插拔的效果。 所以一句话概括就是：为了使程序的扩展性好，易于维护和升级。原文是这样的：Software entities should be open for extension，but closed for modification

 我们举例说明什么是开闭原则，以4s店销售汽车为例，其类图如图所示:

![image-20200731064902417](https://s1.ax1x.com/2020/07/31/aM67RI.md.png)

以下我们以go语言实践如下列子

ICar接口定义了汽车的两个属性：名称和价格。AudiCar是一个奥迪车的实现类，代表奥迪车的总称。Shop4S代表售卖的4S店

ICar接口的代码清单如下

````go
type ICar interface {
	// 车名
	GetName() string

	// 价格
	GetPrice() int
}
````

一般情况下4S店只出售一种品牌的车，这里用奥迪为例，代码清单如下

```go
type AudiCar struct {
	name  string
	price int
}

func (b AudiCar) GetName() string {
	return b.name
}

func (b AudiCar) GetPrice() int {
	return b.price
}
```

这里我们模拟一下4s店售车记录，写个测试

```go
func TestAudi_GetName(t *testing.T) {
	var (
		list []ICar
	)
	list = []ICar{}
	list = append(list, &AudiCar{"A6", 130})
	list = append(list, &AudiCar{"A8", 343})
	list = append(list, &AudiCar{"TT", 60})
	for _, v := range list {
		fmt.Println("车名:", v.GetName(), "\t价格:", v.GetPrice())
	}
}
```

输出的结果

```
=== RUN   TestAudi_GetName
车名: A6 	价格: 130
车名: A8 	价格: 343
车名: TT 	价格: 60
--- PASS: TestAudi_GetName (0.00s)
```

暂时来看，以上设计是没有啥问题的。但是，某一天，4s店老板说奥迪轿车统一要收取一笔金融服务费，收取规则是价格在100万元以上的收取5%，50~100万元的收取2%，其余不收取。为了应对这种需求变化，之前的设计又该如何呢? 目前，解决方案大致有如下三种:

- 修改ICar接口:在ICar接口上加一个getPriceWithFinance接口，专门获取加上金融服务费后的价格信息。这样的后果是，实现类AudiCar也要修改，业务类Shop4S也要做相应调整。ICar 接口一般应该是足够稳定的，不应频繁修改，否则就失去了接口锲约性了。

- 修改AudiCar实现类:直接修改AudiCar类的getPrice方法，添加金融服务费的处理。这样的一个直接后果就是，之前依赖getPrice的业务模块的业务逻辑就发生了改变了，price也不是之前的price了。

- 使用子类拓展来实现:增加子类FinanceAudiCar,覆写父类AudiCar的getPrice方法，实现金融服务费相关逻辑处理。这样的好处是:只需要调整Shop4S中的静态模块区中的代码，main中的逻辑是不用做任何修改的。

定义扩展类代码清单

```go
type FinanceAudiCar struct {
	AudiCar
}

func (b FinanceAudiCar) GetPrice() int {
	// 获取原价
	selfPrice := b.price
	var finance int
	if selfPrice >= 100 {
		finance = selfPrice + selfPrice*5/100
	} else if selfPrice >= 50 {
		finance = selfPrice + selfPrice*2/100
	} else {
		finance = selfPrice
	}
	return finance
}
```

测试类

```go

func TestFinanceAudiCar_GetName(t *testing.T) {
	var (
		list []ICar
	)
	list = []ICar{}
	list = append(list, &FinanceAudiCar{AudiCar{"A6", 130}})
	list = append(list, &FinanceAudiCar{AudiCar{"A8", 343}})
	list = append(list, &FinanceAudiCar{AudiCar{"TT", 60}})
	for _, v := range list {
		fmt.Println("车名:", v.GetName(), "\t价格:", v.GetPrice())
	}
}
```

输出结果

```
=== RUN   TestFinanceAudiCar_GetName
车名: A6 	价格: 136
车名: A8 	价格: 360
车名: TT 	价格: 61
--- PASS: TestFinanceAudiCar_GetName (0.00s)
```

这样，在业务规则发生改变的情况下，我们通过拓展子类及修改持久层(高层次模块)便足以应对多变的需求。开闭原则要求我们尽可能通过拓展来实现变化，尽可能少地改变已有模块，特别是底层模块。

开闭原则总结:

- 提高代码复用性 
- 提高代码的可维护性





#### 参考文章

- [https://github.com/qq570850096/awesome-go-datastruct/blob/master/DesignPatterns/%E4%B8%83%E5%A4%A7%E8%AE%BE%E8%AE%A1%E5%8E%9F%E5%88%99.md](https://github.com/qq570850096/awesome-go-datastruct/blob/master/DesignPatterns/七大设计原则.md)

