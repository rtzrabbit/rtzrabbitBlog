---
title: Java Scanner入门
date: 2019-12-17 10:49:23
categories: Java
tags: [Java, Scanner]
---

<!-- # z -->

## 概述

`Java Scanner`的简单使用。

<!-- more -->

## 简介

`java.util.Scanner`，主要被我们用来获取用户的输入。其实翻阅文档，可以看到`Scanner`类提供了多种构造方法。本文展示：标准输入流`System.in`和字符`String`串两种。

![多种构造方法](http://rtzrabbit.nitmoon.com/e738beeb8a6b5a7822bfe0c6c911bd5a.png)

## 常用方法

### next()

将下一个值作为字符串返回。

```java
Scanner scan = new Scanner(System.in);
// 从键盘接收数据
// next方式接收字符串
System.out.println("next方式接收：");
// 判断是否还有输入
if (scan.hasNext()) {
    String str1 = scan.next();
    System.out.println("输入的数据为：" + str1);
}
scan.close();
```

```bash
next方式接收
$ 神仙？ 妖怪？ 谢谢…
输入的数据为：神仙？
```

### nextLine()

将回车前的全部内容作为字符串返回。

```java
Scanner scan = new Scanner(System.in);
// 从键盘接收数据
// nextLine方式接收字符串
System.out.println("nextLine方式接收：");
// 判断是否还有输入
if (scan.hasNextLine()) {
    String str = scan.nextLine();
    System.out.println("输入的数据为：" + str);
}
scan.close();
```

```bash
nextLine方式接收：
$ 神仙？ 妖怪？ 谢谢…
输入的数据为：神仙？ 妖怪？ 谢谢…
```

### 区别

* next()
  * 一定要读取到有效字符后才可以结束输入。直接输入空格、回车，都不能结束输入。
  * 对输入有效字符之前遇到的空白（空格、回车），`next()` 方法会自动将其去掉。
  * 只有输入有效字符后才将其后面输入的空白作为分隔符或者结束符。
  * `next()` 不能得到带有空格的字符串。
* nextLine()
  * 以Enter为结束符,也就是说 `nextLine()`方法返回的是输入回车之前的所有字符。
  * 可以获得空白。

### nextInt()

将下一个值作为`int`返回，如果不是`int`类型则报错。

```java
Scanner scan = new Scanner(System.in);
// 从键盘接收数据
// nextInt方式接收字符串
System.out.println("nextInt方式接收：");
int i = scan.nextInt();
System.out.println("输入的数据为：" + i);
scan.close();
```

```bash
nextInt方式接收：
$ 123
输入的数据为：123
```

```bash
nextInt方式接收：
$ abc
Error
```

所以使用之前最好判断一下是否为`int`类型

```java
Scanner scan = new Scanner(System.in);
// 从键盘接收数据
// nextInt方式接收字符串
System.out.println("nextInt方式接收：");
// 判断输入是否为int
if (scan.hasNextInt()) {
    int i = scan.nextInt();
    System.out.println("输入的数据为：" + i);
}
scan.close();
```

### 总结

**从上面的示例中可以总结出，在使用输入之前最好使用`hasNextXxx()` 方法进行验证，再使用 `nextXxx()` 来读取。**



## Tips

`Scanner`对象在调用方法时会阻塞程序的运行。例如：

```java
Scanner scan = new Scanner(System.in);
// 从键盘接收数据
// next方式接收字符串
System.out.println("next方式接收：");
// 判断是否还有输入
// 在此处阻塞，等待用户输入
if (scan.hasNext()) {
    String str1 = scan.next();
    System.out.println("输入的数据为：" + str1);
}
scan.close();
```

那么`Scanner`对象在什么时候会阻塞？先从标准输入流`System.in`开始。

如果使用标准输入流`System.in`来作为参数，实例化`Scanner`对象的话，那么在调用`hasNextXxx()`或者`nextXxx()`时就会阻塞，因为此时输入为空。用户输入之后，`hasNextXxx()`将会从用户输入的左边开始判断，也就是用户输入的最开始。如果验证通过，则返回`true`，否则返回`false`。`hasNextXXX()`方法只会判断游标当前指向位置的内容符不符合验证规则，并不会使游标指向下一个。`nextXxx()`方法会使游标后移。

怎么理解游标？**就可以理解为箭头。**

举例：

```java
Scanner scan = new Scanner(System.in);
        
if (scan.hasNext()) {
    System.out.println("haxNext() => true");
} else {
    System.out.println("haxNext() => false");
}

if (scan.hasNextLine()) {
    System.out.println("hasNextLine() => true");
} else {
    System.out.println("hasNextLine() => false");
}

if (scan.hasNextInt()) {
    System.out.println("hasNextInt() => true");
} else {
    System.out.println("hasNextInt() => false");
}

if (scan.hasNextFloat()) {
    System.out.println("hasNextFloat() => true");
} else {
    System.out.println("hasNextFloat() => false");
}

System.out.println("end");

scan.close();
```

```bash
$ abc 111.1 222 333 
abc 111.1 222 333 
haxNext() => true
hasNextLine() => true
hasNextInt() => false
hasNextFloat() => false
end
```

```bash
$ 111.1 abc 222 333 
haxNext() => true
hasNextLine() => true
hasNextInt() => false
hasNextFloat() => true
end
```

```bash
$ 222 abc 111.1 333
haxNext() => true
hasNextLine() => true
hasNextInt() => true
hasNextFloat() => true
end
```

上面的代码只有`hasNextXxx()`方法，没有`nextXxx()`。所以每次判断都是从最左边开始判断。

注意`111.1` `hasNextInt() => false`，`222` `hasNextFloat() => true`.

那么什么是游标后移呢？

举例：

```java
Scanner scan = new Scanner(System.in);
        
System.out.println("next()=>" + scan.next());
System.out.println("nextInt()=>" + scan.nextInt());
System.out.println("nextFloat()=>" + scan.nextFloat());
System.out.println("nextLine()=>" + scan.nextLine());

System.out.println("end");

scan.close();
```

```bash
$ hhello 111 222.2 2333 world 666!
next()=>hello
nextInt()=>111
nextFloat()=>222.2
nextLine()=> 2333 world 666!
神仙？ 妖怪？ 谢谢…
next()=>神仙？
end

```

为了方便讲解，输入的数据以及安装解析的方式故意安排好了。

**注意：因为空格显示格式问题，下面的`-111`表示`空格111`**

执行`next()`方法后，取出`hello`，此时游标后移，指向`-111 222.2 2333 world 666!`

执行`nextInt()`方法后，取出`111`，这个方法会忽略`-111`前面的空格，此时游标后移，指向`-222.2 2333 world 666!`

执行`nextFloat()`方法后，取出`222.2`，这个方法会忽略`-222.2`前面的空格，此时游标后移，指向`-2333 world 666!`

执行`nextLine()`方法后，取出回车前的全部内容`-2333 world 666!`。

此时输入内容彻底被清空，**但是末尾的`end`却没有输出！说明程序此时还在阻塞。**

于是此时再次输入`神仙？ 妖怪？ 谢谢…`

执行`next()`方法后，取出`神仙？`，此时游标后移，指向`-妖怪？ 谢谢…`

但是接下来再也没有代码去取输入了。程序结束。

## 作业

了解了这么多之后，这次的作业怎么做呢？以上教程可以知道如果想要获得用户的输入那么用`nextXxx()`即可获取，但是除了`nextLine()`方法可以一次取出回车前的内容之外，其他的都只能取出 一个数据！如果使用`while`循环，使用`hasNextXxx()`方法作为判断条件，就可以获得无限输入的能力！不过由此也带来了新的问题，程序将陷入无限循环，无法跳出！

```java
while(scan.hasNextInt()) {
    ...
}
```

如果输入其他字符作为结束，输入`2 1 3 end`使用非数字作为结束，即可跳出循环！此时也可以，不过并不完美！作为一个追求完美的程序员，这种方法实在是太膈应！

回头翻看文档，发现`Scanner`类接收`String`类型作为参数，由此灵光一闪！有了新的思路！

使用两个`Scanner`对象，将第一个输入值使用`nextLine()`全部取出，作为第二个对象的输入，再使用循环遍历即可得到全部输入值，而且游标指向空时，`hasNextXxx()`将返回false。nice!完美完成作业！附上代码：

```java
package com.nitmoon.basic;

import java.util.Scanner;

public class InputNumberMax {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		System.out.println("请输入数字：");
		// 声明sc1
		Scanner sc1 = new Scanner(System.in);
		// 取出回车之后输入的所有内容
		String str = sc1.nextLine();
		// 声明sc2，并将sc1的返回值作为输入
		Scanner sc2 = new Scanner(str);
		// max记录最大值，j标记第几个，i计数
		int max = 0, j = 0, i = 0;
		// 循环
		while (sc2.hasNextInt()) {
			i++;
			// 取出int类型的数据
			int in = sc2.nextInt();
			// 初始化max，j，i，只执行1次
			if (i == 1) {
				j++;
				max = in;
				continue;
			}
			// 如果比较大
			if (in > max) {
				j = i;
				max = in;
			}
		}
		System.out.println("最大值为：" + max + "，位于第" + j + "位");
		sc1.close();
		sc2.close();
	}
}

```



