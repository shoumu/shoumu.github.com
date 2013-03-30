---
layout: post
title: "makefile语法"
description: "about makefile"
category: linux c
tags: [makefile, linux c]
---
{% include JB/setup %}

用makefile来进行工程的控制是一个非常方便的手段。这边文章就作为自己学习makefile
的一个简单的笔记。

### 基本语法

首先需要建立一个名字为makefile的文件。

然后在文件中写入具体的内容。格式为：

	文件名:文件名 （文件名……）
		命令

如果冒号后面的文件比前面的文件新的话，那么就会执行下面的命令，
更新冒号前面的文件。

实例：
	
	fork:fork.o
		cc -o fork fork.o

	fork.o:fork.c
		cc -c fork.c

	clean:
		rm fork fork.c

### 文件的依赖结构

一个makefile文件应该可以唯一地构成一棵文件树，如果存在多课文件树的话，
那么只会对第一棵起作用。比如说：

	fork:fork.o
		cc -o fork fork.o

	signal:signal.o
		cc -o signal signal.o

	fork.o:fork.c
		cc -c fork.c

	signal.o:signal.c
		cc -c fignal.c

	clean:
		rm fork fork.o signal signal.o

在上面的这个例子中，可以看到，fork和signal之间是没有任何关系，上面的结构就能够构成
两棵文件树。所以当使用make命令的时候就之能够编译fork，因为fork在上面。

在makefile文件最后的是clean，这个的作用就是调用make clean是完成的工作。我们就可以
在这个地方将中间结果清楚，很方便。

### makefile中使用变量

当工程比较小的时候，我们上面那样使用是非常简单的事情，但是当工程非常大的时候，
可能就不会那样轻松了，就比如.o文件，我们就可能会在多个地方使用到。比如：
	
	edit:main.o command.o display.o
		cc -o edit main.o command.o display.o

	clean:
		rm main.o command.o display.o

上面那样，我们连续写了两次，就难免有时候可能会犯错，也是比较麻烦的事情，所以
这个时候使用变量的话就会轻松许多了啊。如下：

	object = main.o command.o display.o

引用的时候只要使用“$(变量名)”就可以了。

	edit:$(object)
		cc -o edit $(object)

	clean:
		rm $(object)

### make自动推导

make具有非常强大的功能，能够自动推导文件的依赖关系。比如前面有了name.o文件，那后面
就可以不用再写name.o的规则了，make能够自动推导出name.o对name.c的依赖。比如：

	edit:name.o
		cc -o edit name.o

上面就能够自动推导出：
	
	name.o:name.c
		cc -c name.c

### .PHONY规则

首先看下面的一种做法：

	.PHONY:clean
	clean:
		-rm name.o

.PHONY意思表示clean是一个“伪目标”，。而在rm命令前面加了一个小减号的意思就是，也许
某些文件出现问题，但不要管，继续做后面的事。

----

参考地址：[http://blog.csdn.net/liang13664759/article/details/1771246](http://blog.csdn.net/liang13664759/article/details/1771246)


