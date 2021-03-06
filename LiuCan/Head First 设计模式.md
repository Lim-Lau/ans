---
title: Head First 设计模式
tags: 设计模式,HeadFirst,java
renderNumberedHeading: true
grammar_cjkRuby: true
---

**基础**

>抽象
>封装
>多型
>继承


**设计原则**
 > 1. 把会变化的部分取出并封装起来，以便以后可以轻易地改动或扩充此部分，而不影响不需要变化的其他部分；（封装变化）
 > 
 > 2. 针对”接口“编程而不是针对实现编程；（针对超类型编程）
 > 
 > 3. 多用组合，少用继承；
 > 
 > 4. 为了交互对象之间的松耦合设计而努力；
 > 
 > 5.开放关闭原则：类应该对拓展开放，对修改关闭。（具有弹性可以应对改变，可以接受新的功能来应对改变的需求。）

**策略模式**

> 定义了算法族，分别封装起来，让他们之间互相替换，此模式让算法的变化独立于使用算法的客户。

**观察者模式**
>出版者（主题 Subject）+订阅者（观察者 Observer）=观察者模式
>定义了对象之间的一对多依赖，这样一来，当一个对象改变状态时，它的所有依赖者都会收到通知并自动更新
>java.util.Observable的黑暗面
	>Observable是一个类
		>因为是个类，就必须在设计时继承于它，如果某个类想同时具有Observable和其他一个超类的行为，就陷入两难，毕竟java不支持多继承。这限制了Observable的复用潜力。
	>Observable不是个接口，无法自己独立实现和Java内置的Observer API 搭配使用，也无法将java.until的实现换成另一套做法的实现（比如：Observable 将关键的方法保护起来，意味着：除非继承Observable，否则无法创建Observable实列并组合到自己的对象中来），这违反了第二设计原则：“多用组合，少用继承”。
	
 **装饰者模式**
 >动态地将责任附加到对象上。若要拓展功能，装饰者提供了比继承更有弹性的替代方案。
 >装饰者与被装饰者具有共同的超类；
 >装饰者可以在所委托被装饰者的行为之前与/或之后，加上自己的行为，以达到特定的目的；
 >装饰者通常是用其他类似于工厂或生成器这样的模式创建；
 >装饰者该做的事：增加行为到被包装对象上。

**工厂模式**
>工厂方法用来处理对象得创建，并将这样的行为封装在子类中，这样客户程序中关于超类的代码就和子类对象创建代码解耦。
> 工厂方法模式：定义了一个创建对象的接口，但是由子类决定要实例化的类是哪一个，工厂方法让类把实例化推迟到子类。