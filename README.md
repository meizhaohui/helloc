# C程序设计语言总结

- 注：实验环境Manjaro发行版和Windows 10系统。

Manjaro系统：
```shell
[mzh@manjaro helloc]$ uname -r
4.19.28-1-MANJARO
```

Windows 10系统：
```cmd
$ uname -a
MSYS_NT-10.0 LAPTOP-8OESE8K5 2.5.0(0.295/5/3) 2016-03-31 18:47 x86_64 Msys  
```

- 学习一门新程序设计语言的唯一途径就是使用它编写程序。
- 总结主要基于《C程序设计语言 第2版新版 徐宝文译》一书。

## 第1章 导言
### 第一个程序hello world

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
- \\"转义字符表示双引号。
- \\\\转义字符表示反斜杠本身。

### 变量与算术表达式

打印华氏温度与摄氏温度对照表，对应关系为 ```C=(5/9)(F-32)```。

```c
[mzh@manjaro source]$ cat fahrenheit2celsius.c
/**
*@file fahrenheit2celsius.c
*@brief 打印华氏温度与摄氏温度对照表
*@author Zhaohui Mei<mzh.whut@gmail.com>
*@date 2019-07-24
*@return 0
*/

#include <stdio.h>

int main()
{
    int fahr, cels;  // 声明变量
    int lower, upper, step;  // 声明变量

    lower = 0;
    upper = 300;
    step = 20;

    fahr = lower;
    while (fahr <= upper)
    {
        cels = 5 * (fahr - 32) / 9;
        printf("%d\t%d\n", fahr, cels);
        fahr = fahr + step;
    }
    return 0;
}
```

生成输出文件```fahrenheit2celsius.out ``` 并运行：

```shell
[mzh@manjaro source]$ ./fahrenheit2celsius.out 
0	-17
20	-6
40	4
60	15
80	26
100	37
120	48
140	60
160	71
180	82
200	93
220	104
240	115
260	126
280	137
300	148
```

- 注释：包含在``/*``和``*/``之间的字符序列将被编译器忽略。注释可以自由地运用在程序中，使得程序更易于理解。也可以使用```//```来表示单行注释。
- 所有变量必须先声明后使用。声明通常放在函数起始处，在任何可执行语句之前。声明用于说明变量的属性，它由一个类型名和一个变量名组成。
- 赋值语句：类似```lower = 0```这样使用等号对变量进行赋值。

### ```while```语句

- ```while```循环语句中，首先测试圆括号中的条件 ，如果条件为真，则执行循环体，然后再重复测试圆括号中的条件，如果为真，则再次执行循环体；当圆括号中的条件测试为假时，循环结束。
- ```printf```函数不是C语言本身的一部分，C语言本身并没有定义输入/输出功能。```printf```是标准库中定义的一个函数。

上面温度转换存在两个问题，输出不是右对齐，不够美观；摄氏温度的精确度不够。我们进行优化：

```c
[mzh@manjaro source]$ cat fahrenheit2celsius.c
/**
*@file fahrenheit2celsius.c
*@brief 打印华氏温度与摄氏温度对照表
*@author Zhaohui Mei<mzh.whut@gmail.com>
*@date 2019-07-24
*@return 0
*/

#include <stdio.h>

int main()
{
    float fahr, cels;  // 声明变量
    int lower, upper, step;  // 声明变量

    lower = 0;
    upper = 300;
    step = 20;
    
    printf("  F      C\n");
    fahr = lower;
    while (fahr <= upper)
    {
        cels = 5.0 * (fahr - 32.0) / 9.0;
        printf("%3.0f %6.1f\n", fahr, cels);  // 华氏温度取三位字符宽，不带小数点和小数部分，摄氏温度取六位字符宽，且小数点后面取1位小数
        fahr = fahr + step;
    }
    return 0;
}
```

生成输出文件```fahrenheit2celsius.out ``` 并运行：

```shell
[mzh@manjaro source]$ ./fahrenheit2celsius.out 
  F      C
  0  -17.8
 20   -6.7
 40    4.4
 60   15.6
 80   26.7
100   37.8
120   48.9
140   60.0
160   71.1
180   82.2
200   93.3
220  104.4
240  115.6
260  126.7
280  137.8
300  148.9
```

- 使用浮点算术运算代替整型算术运算，控制输出精度。
- 如果浮点常量取值是整型值，在书写时最好为它加上一个显示的小数点，这样可以强调其是浮点性质，便于阅读。
- 可以使用```printf```另外单独打印标题。

### ```for```语句

使用```for```循环实现上面的功能：

```c
[mzh@manjaro source]$ cat fahrenheit2celsius_for.c
/**
*@file fahrenheit2celsius_for.c
*@brief 打印华氏温度与摄氏温度对照表
*@author Zhaohui Mei<mzh.whut@gmail.com>
*@date 2019-07-24
*@return 0
*/

#include <stdio.h>

int main()
{
    float cels;  // 声明变量

    printf("  F      C\n");
    for(int fahr = 0;fahr <= 300;fahr = fahr + 20)
    {
        cels = 5.0 * (fahr - 32.0) / 9.0;
        printf("%3d %6.1f\n", fahr, cels);  // 华氏温度取三位字符宽，不带小数点和小数部分，摄氏温度取六位字符宽，且小数点后面取1位小数
    }
    return 0;
}
```

生成输出文件```fahrenheit2celsius_for.out ``` 并运行：

```shell
[mzh@manjaro source]$ ./fahrenheit2celsius_for.out 
  F      C
  0  -17.8
 20   -6.7
 40    4.4
 60   15.6
 80   26.7
100   37.8
120   48.9
140   60.0
160   71.1
180   82.2
200   93.3
220  104.4
240  115.6
260  126.7
280  137.8
300  148.9
```

- 上面使用```for```的结果与使用```while```语句的结果相同。
- ```for```循环语句适合于初始化和增加步长都是条件语句并且逻辑相关的情形，比```while```语句更加紧凑。

以逆序打印转换表：

```c
[mzh@manjaro source]$ cat fahrenheit2celsius_for.c
/**
*@file fahrenheit2celsius_for.c
*@brief 打印华氏温度与摄氏温度对照表
*@author Zhaohui Mei<mzh.whut@gmail.com>
*@date 2019-07-24
*@return 0
*/

#include <stdio.h>

int main()
{
    float cels;  // 声明变量

    printf("  F      C\n");
    for(int fahr = 300;fahr >= 0;fahr = fahr - 20)
    {
        cels = 5.0 * (fahr - 32.0) / 9.0;
        printf("%3d %6.1f\n", fahr, cels);  // 华氏温度取三位字符宽，不带小数点和小数部分，摄氏温度取六位字符宽，且小数点后面取1位小数
    }
    return 0;
}
```

输出如下：

```shell
[mzh@manjaro source]$ ./fahrenheit2celsius_for.out 
  F      C
300  148.9
280  137.8
260  126.7
240  115.6
220  104.4
200   93.3
180   82.2
160   71.1
140   60.0
120   48.9
100   37.8
 80   26.7
 60   15.6
 40    4.4
 20   -6.7
  0  -17.8
```

### ```#define```定义符号常量

- 在程序中使用300或20等类似的```幻数```不是一个好习惯。它们几乎无法向以后阅读该程序的人提供什么信息，而且使程序的修改变得更加困难。处理幻数的一种方法就是赋予它们有意义的名字。
- ```#define```指令可以把符号名(或称为符号常量)定义为一个特定的字符串，语法格式如下：

​       ```#define 名字 替换文本```

- 程序中所有出现在在```#define```中定义的符号常量的名字都将被相应的替换文本替换。

下面对温度转换表程序进行优化，使用符号常量：

```c
[mzh@manjaro source]$ cat fahrenheit2celsius_for_define.c 
/**
*@file fahrenheit2celsius_for_define.c
*@brief 打印华氏温度与摄氏温度对照表
*@author Zhaohui Mei<mzh.whut@gmail.com>
*@date 2019-07-24
*@return 0
*/

#include <stdio.h>

#define LOWER 0  // 温度表的下限
#define UPPER 300  // 温度表的上限
#define STEP 20  // 步长
int main()
{
    float cels;  // 声明变量

    printf("  F      C\n");
    for(int fahr = LOWER;fahr <= UPPER;fahr = fahr + STEP)
    {
        cels = 5.0 * (fahr - 32.0) / 9.0;
        printf("%3d %6.1f\n", fahr, cels);  // 华氏温度取三位字符宽，不带小数点和小数部分，摄氏温度取六位字符宽，且小数点后面取1位小数
    }
    return 0;
}
```

生成输出文件```fahrenheit2celsius_for_define.out ``` 并运行：

```shell
[mzh@manjaro source]$ cc fahrenheit2celsius_for_define.c -o fahrenheit2celsius_for_define.out
[mzh@manjaro source]$ ./fahrenheit2celsius_for_define.out 
  F      C
  0  -17.8
 20   -6.7
 40    4.4
 60   15.6
 80   26.7
100   37.8
120   48.9
140   60.0
160   71.1
180   82.2
200   93.3
220  104.4
240  115.6
260  126.7
280  137.8
300  148.9
```

示例中定义了```LOWER```,```UPPER```,```STEP```三个符号常量。

- 符号常量不是变量，不需要出现在声明中。
- 符号常量通常使用大写字母拼写，这样很容易与小写字母拼写的变量名相区别。
- ```#define```指令定义符号常量的行的末尾**不需要分号结尾**！

### ```getchar()```,```putchar()```字符输入与输出

- 无论文本从可处输入，输出到何处，其输入，输出都是按照字符流的方式处理。。
- 文本流是由多行字符构成的字符序列，而每行字符则是由0个或多个字符组成，行末是一个换行符。
- 标准库负责使每个输入/输出流都 能够遵守这一模型。
- 标准库提供了一次读/写一个字符的函数，其中最简单的是```getchar()```,```putchar()```两个函数。
- ```getchar()```每次从文本流中读入下一个输入字符，并将其作为结果值返回。
- ```putchar(c)```将打印变量c的内容，通常是显示在屏幕上。

在不了解其他输入/输出知识的情况下，可以使用```getchar()```,```putchar()```函数编写出数量惊人的有用代码。

#### 将输入流原样显示在输出流

每读入一个字符后，就把这个字符原样显示在输出流中，如下示例：

```c
[mzh@manjaro source]$ cat getcharputchar.c
/**
*@file getcharputchar.c
*@brief 将输入字符复制到输出 
*@author Zhaohui Mei<mzh.whut@gmail.com>
*@date 2019-07-24
*@return 0
*/

#include <stdio.h>

int main()
{
    int c;

    c = getchar();
    while(c != EOF)
    {
        putchar(c);
        c = getchar();
    }
    return 0;
}
```

编译并运行：

```shell
[mzh@manjaro source]$ cc getcharputchar.c -o getcharputchar.out
[mzh@manjaro source]$ ./getcharputchar.out 
a
a
b
b
c
c
```

运行时，我输入字母a，马上就输出字母a，我再输出字母b，马上就输出字母b，我再输出字母c，马上就输出字母c，我按Ctrl-D终止，此时程序就自动退出了。

- Linux中，在新的一行的开头，按下Ctrl-D，就代表EOF（如果在一行的中间按下Ctrl-D，则表示输出"标准输入"的缓存区，所以这时必须按两次Ctrl-D）。参考[EOF是什么？](http://www.ruanyifeng.com/blog/2011/11/eof.html "EOF是什么？")
- 字符在键盘，屏幕或其他任何地方无论以什么形式表现，它在机器内部都是以位模式存储的。```char```类型专门用于存储这种字符型数据，当然任何整型(```int```)也可以用于存储字符型数据，出于某些潜在的重要原因，在此使用```int```类型。
- ```EOF```表示文件结束，End of file。定义在头文件```stdio.h```中 ，定义如下``` #define EOF (-1)```。



使用赋值语句形成复杂语句实现同样的功能：

```c
[mzh@manjaro source]$ cat getcharnotequaleof.c
/**
*@file getcharnotequaleof.c
*@brief 使用赋值语句  
*@author Zhaohui Mei<mzh.whut@gmail.com>
*@date 2019-08-01
*@return 0
*/

#include <stdio.h>

int main()
{
    int c;
    while ((c = getchar()) != EOF)
    {
        putchar(c);
    }
    return 0;
}
```

#### 字符统计

下面统计输入了多少个字符：

```c
[mzh@manjaro source]$ cat countchar.c
/**
*@file countchar.c
*@brief 统计字符数 
*@author Zhaohui Mei<mzh.whut@gmail.com>
*@date 2019-08-01
*@return nc
*/

#include <stdio.h>

int main()
{
    long nc = 0;
    while (getchar() != EOF)
    {
        ++nc;
    }
    printf("%ld\n", nc);
    return nc;
}

```

编译并运行：

```shell
[mzh@manjaro source]$ cc countchar.c -o countchar.out 
[mzh@manjaro source]$ ./countchar.out 
a
b
c
d
8
```

由于输入字符a/b/c/d后都按了一次Enter回车键，所以每行相当于输入了两个字符，一共输入了8个字符。

#### 行统计

下面统计输入的行数：

```c
[mzh@manjaro source]$ cat countline.c
/**
*@file countline.c
*@brief 统计行数 
*@author Zhaohui Mei<mzh.whut@gmail.com>
*@date 2019-08-01
*@return nl
*/

#include <stdio.h>

int main()
{
    int c = 0;
    int nl = 0;
    while ((c = getchar() ) != EOF)
    {
        if (c == '\n')
        {
            ++nl;
        }
    }
    printf("lines: %d\n", nl);
    return nl;
}
```

编译并执行：

```shell
[mzh@manjaro source]$ cc countline.c -o countline.out
[mzh@manjaro source]$ ./countline.out 
ab
cd
ef
gh
lines: 4
```

一共输入了四行，最后按Ctrl+D时就会打印行数：lines:4 。

#### 将连续的多个空格用一个空格代替

练习1-9：编写一个将输入复制到输出的程序，并将其中连续的多个空格用一个空格代替。

```c
[mzh@manjaro source]$ cat replacemultispace2onespace.c
/**
*@file replacemultispace2onespace.c
*@brief 将输入复制到输出，并将连接多个空格用一个空格代替 
*@author Zhaohui Mei<mzh.whut@gmail.com>
*@date 2019-08-01
*@return 0 
*/

#include <stdio.h>

int main()
{
    int c;
    c = getchar(); // 读取字符
    while (c != EOF)  // 不是文件结尾符时
    {
        if (c == ' ')  // 如果读取到的字符时空格
        {
            putchar(c); // 先将空格打印出来
            // 然后循环读取后面的字符，如果字符是空格，不打印
            // 如果不是空格，则跳出内层while语句
            while((c=getchar()) == ' ' && c != EOF)
            {
                ;
            }
        }
        putchar(c); // 打印内层while中检查到的非空格字符
        c = getchar(); // 再读取下一个字符
    }
    return 0;
}
```

编译并运行：

```shell
[mzh@manjaro source]$ cc replacemultispace2onespace.c -o replacemultispace2onespace.out
[mzh@manjaro source]$ ./replacemultispace2onespace.out 
aa bb   cc   ddd   eeee      fff    ggg
aa bb cc ddd eeee fff ggg
```

#### 将制表符用"\t"代替显示

练习1-10：编写一个将输入复制到输出的程序，并将其中的制表符替换为\t，将反斜杠替换为\\\。

```c
[mzh@manjaro source]$ cat replacechars.c
/**
*@file replacechars.c
*@brief 输入复制到输出，并替换多个字符 
*@author Zhaohui Mei<mzh.whut@gmail.com>
*@date 2019-08-01
*@return 0
*/

#include <stdio.h>

int main()
{
    int c;
    while((c = getchar()) != EOF)
    {
        if (c == '\t')
            printf("\\t");
        if (c == '\b')
            printf("\\b");
        if (c == '\\')
            printf("\\\\");
        if ((c != '\t') && (c != '\b') && (c != '\\'))
            putchar(c);
    }
    return 0;
}
```

编译并运行：

```shell
[mzh@manjaro source]$ ./replacechars.out 
a	b       c
a\tb\tc
a\b\c
a\\b\\c
```

#### 统计文件的行数、单词数、字符数

下面的程序用于统计统计行数、单词数和字符数，此处对单词的定义比较宽松，它是任何其中不包含空格、换行符和制表符的字符序列。下面的程序功能类似于linux上的命令 ``wc`` 。

```c
[mzh@manjaro source]$ cat count_words.c
/**
*@file count_words.c
*@brief 统计输入的行数、单词数与字符数
*@author Zhaohui Mei<mzh.whut@gmail.com>
*@date 2019-10-20
*@return 0
*/

#include <stdio.h>

#define IN 1  /* 定义符号常量，在单词中 */
#define OUT 0  /* 定义符号常量，不在单词中 */

int main()
{
    int c, nl, nw, nc, state;  /* 字符、行数、单词数、字符数、状态信息 */
    
    state = OUT;  /* 初始状态不在单词中 */
    nl = nw = nc = 0;  /* 行数、单词数、字符数初始值为0 */
    
    /* 不停地读取字符数据，如果不是结尾符 */
    while((c = getchar()) != EOF )
    {
        ++nc;  /* 字符数增1 */
        if (c == '\n')
        {
            ++nl; /* 行数增1 */
        }
        /* 如果字符是空格或是换行符或是制表符*/
        if (c == ' ' || c == '\n' || c == '\t')
        {
            state = OUT;
        }
        else if (state == OUT )
        {
            state = IN;
            ++nw;  /* 单词数增1 */
        }
    }
    printf("%d %d %d\n", nl, nw, nc);
    return 0;
}
```


编译并运行：

```shell
[mzh@manjaro source]$ cc count_words.c -o count_words.out
[mzh@manjaro source]$ ./count_words.out < count_words.c
43 124 1076
```

注：第1章后面讲的数组、函数、参数--传值调用、字符数组、外部变量与作用域，相对较难，后续补充。

## 第2章 类型、运算符与表达式

- 变量和常量是程序处理的两种基本数据对象。
- 声明语句说明变量的名字及类型，也可以指定变量的初值。
- 运算符指定将要进行的操作。
- 表达式则把变量与常量组合起来生成新的值。
- 对象的类型决定该对象可取值的集合以及可以对该对象执行的操作。
- 所有整型都包含``signed(带符号)``和``unsigned(无符号)``两种形式。
- 对象可以声明为``const(常量)``类型，表明其值不能修改。

### 变量名的限定

- 对常量的命名与符号常量的命名存在一些限定条件。
- 名字是由字母(a-zA-Z)和数字(0-9)组成的序列，下划线(_)被看作字母，名字开头不能是下划线。
- 使用下划线可以提高变量名的可读性。
- 名字大小写敏感的。
- 变量名使用使用小写字母(lower_case_with_underline)，符号常量名全部使用大写字母(UPPER_CASE_WITH_UNDERLINE)。
- 名称要能够尽可能从字面上表达变量的用途。

### 数据类型及长度

使用下面的图形表示C语言数据类型(注：MarkDown图形的绘制参考https://blog.csdn.net/whatday/article/details/88655461)：

```mermaid
graph LR;
  C语言数据类型-->基本类型
  C语言数据类型-->指针类型
  C语言数据类型-->空类型void
  C语言数据类型-->构造类型
  基本类型-->整型
  基本类型-->浮点型
  基本类型-->字符类型char
  整型-->短整型short
  整型-->整型int
  整型-->长整型long
  浮点型-->单精度浮点型float
  浮点型-->双精度浮点型double
  构造类型-->数组
  构造类型-->结构体struct
  构造类型-->共用体union
  构造类型-->枚举类型enum
```

- 此处仅说明基本类型， ``short``、``int``、``long``、``float``、``double``、``char``这六个关键字代表C语言里面的六种基本数据类型。
- 字符类型``char``占用1个字节，可以存放本地字符集中的一个字符。
- 各种类型的存储大小与系统位数有关，但目前通用的以64位系统为主。
- 可以使用``sizeof``获取类型的长度，表达式``sizeof(type)``得到对象或类型的存储字节大小。

使用下面的代码获取``short``、``int``、``long``、``float``、``double``、``char``基本数据类型在Linux和Windows 10系统中的长度：

```shell
$ cat base_data_type.c                                         
/**                                                            
*@file base_data_type.c                                        
*@brief 基本数据类型                                           
*@author Zhaohui Mei<mzh.whut@gmail.com>                       
*@date 2019-10-20                                              
*@return 0                                                     
*/                                                             
                                                               
#include <stdio.h>                                             
#include <limits.h>                                            
                                                               
int main()                                                     
{                                                              
    printf("char占用的内存大小是%d\n", sizeof(char));          
    printf("short占用的内存大小是%d\n", sizeof(short));        
    printf("int占用的内存大小是%d\n", sizeof(int));            
    printf("long占用的内存大小是%d\n", sizeof(long));          
    printf("float占用的内存大小是%d\n", sizeof(float));        
    printf("double占用的内存大小是%d\n", sizeof(double));      
    return 0;                                                  
}                                                              
```

在Linux系统上面编译运行：

```shell
[mzh@manjaro source]$ cc base_data_type.c -o base_data_type.out
[mzh@manjaro source]$ ./base_data_type.out
char占用的内存大小是1
short占用的内存大小是2
int占用的内存大小是4
long占用的内存大小是8
float占用的内存大小是4
double占用的内存大小是8
```

在Windows 10系统上面编译运行：
```cmd
D:\data\github_tmp\helloc\source (master -> origin)   
$ chcp 65001                                          
Active code page: 65001                               
                                                      
D:\data\github_tmp\helloc\source (master -> origin)   
$ cc base_data_type.c -o base_data_type.out           
                                                      
D:\data\github_tmp\helloc\source (master -> origin)   
$ base_data_type.out                                  
char占用的内存大小是1                                 
short占用的内存大小是2                                
int占用的内存大小是4                                  
long占用的内存大小是4                                 
float占用的内存大小是4                                
double占用的内存大小是8                               
```

可以看到``long``长整型在Linux系统和Windows系统上面存在差异。

### 常量

- 类似于1 2 3 4 的整数常量属于``int``类型。
- ``long``长整型类型的常量以字母l或L结尾。
- 没有后缀的浮点数为``double``类型，后缀f或F表示为``float``类型，后缀l或L表示为``long double``类型。
- 无符号常量以字母u或U结尾，后缀ul或UL表明是``unsigned long``类型。
- 前缀为0的整型常量表示它是八进制形式。十进制31可以写成八进制形式037。
- 前缀为0x或0X的整型常量表示它是十六进制形式。十进制31可以写成十六进制形式0x1f或0X1F。
- 一个字符常量是一个整数，书写时将一个字符括在单引号中，如'x'。字符在机器字符集中的数值就是字符常量的值。
- 某些字符可以通过转义字符序列(例如：换行符\n)表示为字符和字符串常量。
- 转义字符序列看起来像两个字符，但只表示一个字符。
- 可以使用'\ooo'表示任意的字节大小的位模式，其中ooo代表1~3个八进制数字(0~7)。
- 也可以使用'\xhh'表示任意的字节大小的位模式，其中hh代表1个或多个十六进制数字(0~9,a~f,A~F)。

下表是ANSI C语言中的全部转义字符序列：

| 转义字符 | ASCII码值(十进制)   |          意义                        |
| -------- | :------------------ | :----------------------------------: |
| \\a      | 007                 | 响铃符(BEL)                          |
| \\b      | 008                 | 退格符(BS)，将当前位置移到前一列     |
| \\f      | 012                 | 换页符(FF)，将当前位置移到下页开头   |
| \\n      | 010                 | 换行符(LF)，将当前位置移到下一行开头 |
| \\r      | 013                 | 回车符(CR) ，将当前位置移到本行开头  |
| \\t      | 009                 | 水平制表符(HT)                       |
| \\v      | 011                 | 垂直制表符(VT)                       |
| \\\\     | 092                 | 反斜杠                               |
| \\\'     | 039                 | 单引号                               |
| \\\"     | 034                 | 双引号                               |
| \ooo     | -                   | 八进制                               |
| \xhh     | -                   | 十六进制                             |

- 字符常量'\0'表示值为0的字符，也就是空字符(null)。
- 通常用'\0'的形式代替0，以强调某些表达式的字符属性，但其数字值为0。
- 常量表达式是仅仅包含常量的表达式。
- 常量表达式在编译时求值，而不是在运行时求值。它可以出现在常量可以出现的任何位置。

常量表达式示例：

```c
#define MAXLINE 1000
#define LEAP 1 // 闰年
char line[MAXLINE+1];
int days[31+28+LEAP+31+30+31+30+31+31+30+31+30+31];
```

#### 字符串常量

- 字符串常量也叫做字符串字面值。
- 字符串常量是用双引号括起来的0个或多个字符组成的字符序列。例如"I am a string"。
- 空字符串常量""。
- 双引号不是字符串的一部分，它只用于限定字符串。
- 编译时可以将多个字符串常量连接起来，例如： "hello," " world"与"hello, world"等价。
- 字符串常量的连接为将较长的字符串分散在若干个源文件行中提供了支持。
- 从技术角度看，字符串常量就是字符数组。
- 字符串的内部表示使用一个空字符'\0'作为字符串的结尾。
- 存储字符串的物理存储单元数比括在双引号中的字符数多一个。
- C语言对字符串的长度没有限制，但程序必须扫描完整个字符串后才能确定字符串的长度。
- 标准库函数``strlen(s)``可以返回字符串参数s的长度，但长度不包括末尾的'\0'。
- 标准头文件<string.h>中声明了strlen和其他字符串函数。

获取字符串的长度:

```shell
$cat base_data_type.c
/**
*@file count_string_length.c
*@brief count string length
*@author Zhaohui Mei<mzh.whut@gmail.com>
*@date 2019-10-20
*@return 0
*/

#include <stdio.h>

// my_strlen函数返回a的长度
int my_strlen(char a[])
{
    int i = 0;
    while(a[i] != '\0')
        ++i;
    return i;
}

int main()
{
    char string[] = "Hello World";
    printf("\"Hello World\"字符串的长度为%d", my_strlen(string));
    return 0;
}
```

在Windows 10系统上面编译运行：
```cmd
D:\data\github_tmp\helloc\source (master -> origin)    
$ cc count_string_length.c -o count_string_length.out  
                                                       
D:\data\github_tmp\helloc\source (master -> origin)    
$ count_string_length.out                              
"Hello World"字符串的长度为11                          
```

- 字符常量与仅包含一个字符的字符串有差别的。
- 'x'和"x"是不同的，'x'是一个整数，其值是字线x在机器字符集中对应的数值；"x"是一个包含一个字符(即字母x)以及一个结尾符'\0'的字符数组。
