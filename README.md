# C程序设计语言总结

- 学习一门新程序设计语言的唯一途径就是使用它编写程序。

## 第一个程序hello world

helloworld.c文件内容如下：

```c
#include <stdio.h>

int main()
{
	printf("Hello,world\n");
}
```

编译：

```shell
cc helloworld.c
```

此时默认输出a.out。

通过下面的方式可以指定输出文件的名称：

```shell
cc helloworld.c -o helloworld.out
```

运行：

```shell
[mzh@manjaro c]$ ./a.out 
Hello,world
[mzh@manjaro c]$ ./helloworld.out 
Hello,world
```

- 一个C语言程序，无论其大小如何，都是由**函数**和**变量**组成的。
- 每个程序都从**main**函数的起点开始执行，每个程序必须在某个位置包含一个**main**函数。
- 函数之间进行数据交换的一种方法是调用函数向被调用函数提供参数列表。
- **printf**函数用于打印输出。**printf**永远不会自动换行。
- 用双引号括起来的字符序列称为**字符串**或**字符常量**。
- **\n**转义字符表示换行。
- **\t**转义字符表示制表符tab。
- **\b**转义字符表示回退符。
- **\\"**转义字符表示双引号。
- **\\\\**转义字符表示反斜杠本身。

