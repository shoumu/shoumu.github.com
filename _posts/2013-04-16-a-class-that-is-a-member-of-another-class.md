---
layout: post
title: C++成员类的初始化
description: ""
category: C++
tags: [C++, 类, 初始化]
---
{% include JB/setup %}

学习C++的时候只是看了C++ Primer，没有好好做练习，所以就欠下了很多的账。其中一个问题就是一个类作为另一个类的成员的时候，这个类的构造函数含有参数，那么要如何进行初始化。
这个问题遇见了几次，不知道以前是怎么处理的，今天又遇到了，还是不知道应该要怎么做，就查了一下资料，下面简单说一下。

#### C++实例化一个类

我们知道C++实例化一个类比较简单，如下：

	class A
	{
	public:
		A();
		A(int)
	private:
	}

	A a;

这里就直接实例化了一个类，其中我们看见A的构造函数中没有参数，如果是有参数的话，可以这样子：

	A a(1);

的确看起来就多方便的样子，那如果是出现了下面的这样子的情况的时候呢？

	class A
	{
	public:
		A(int);
	}

	class B
	{
	public:
		B();

	private:
		A a;
	}

如果是出现了上面的这种情况的时候需要怎么初始化呢，这里a是A的一个成员，就有点难做了。
也一直困扰我。

#### 解决方案1 使用初始化列表

也就是在B的构造函数的地方使用初始化列表，对a进行初始化。需要注意的是，构造函数的初始化列表
中从成员总是在函数体执行之前执行，也就是不能够使用在B构造函数中初始化的数据对A进行初始化，可以使用
B构造函数中传递进来的参数进行初始化：

	class A 
	{
	public:
		A(int);
	}

	class B
	{
	public:
		B(int i):a(i)
		{}
	
	private:
		A a;
	}

#### 解决方案2 使用指针

使用指针的话，我们可以使用new操作，这个时候就可以进行初始化。

	class A
	{
	public:
		A(int)
	}

	class B
	{
	public:
		B(int i);
	
	private:
		A *a;
	}

	B::B(int i)
	{
		a = new A(i);
	}

#### 解决方案3 创建一个初始化函数完成初始化

在类A中，我们可以创建一个initial函数，然后通过这个函数，完成所有参数的初始化。

	class A
	{
	public:
		A();
		void initial(int i);

	private:
		int inner;
	}

	class B
	{
	public:
		B(int i);

	private:
		A a;
	}

所有定义如上，然后可以实现：
	
	void A::initial(int i)
	{
		inner = i;
	}

	B::B(int i)
	{
		a.initial(i);
	}

这样就可以实现a中数据的初始化工作了。

#### 写在后面

上面的解决方案1中的初始化列表写得十分简单，但是初始化列表是一个十分强大的功能，这一块还需要查阅资料，补充一下。

有问题一定要及早的解决，不然下一次遇到的时候就可以能会浪费更多的时间，这样子划不来。
