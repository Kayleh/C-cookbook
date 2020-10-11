# 用函数实现模块化程序设计

>Realize modular programming with functions

用函数输出以下结果：

```c
*****************
How do you do!
*****************
```

```c
#include<stdio.h>

int main()
{
    void print_star();//函数的声明
    void print_message();//函数的声明
    print_star();
    print_message();
    print_star();
    return 0;
}

void print_star()
{
    printf("*****************\n");
}

void print_message()
{
    printf("How do you do!\n");
}
```

> 函数声明的作用是把有关函数的信息(函数名,函数类型,函数参数的个数与类型)通知编译系统,以便在编译系统对程序进行编译时,在进行到main函数调用print_star()和print_message()时知道它们是函数而不是变量或其他对象,还对调用函数的正确性进行检查.
>
> 函数间可以互相调用,但不能调用main函数,main函数是被操作系统调用的.
>
> 函数从用户使用的角度看分为:
>
> - 库函数
> - 用户自己定义的函数
>
> 从函数形式分为:
>
> - 无参函数
> - 有参函数

## 怎么定义函数

定义函数应包括哪些内容:

- 函数的名字,方便按名调用
- 函数的类型,即函数返回值的类型
- 函数的参数的名字和类型,调用函数时需要传递的数据
- 函数的功能,函数体

#### 定义无参函数

```c
类型名	函数名()
{
    函数体
}
或
类型名	函数名(void)
{
    函数体
}
```

函数体包括声明部分(变量)和语句部分.

#### 定义有参函数

```c
类型名	函数名(形式参数表列)
{
    函数体
}
```

#### 定义空函数

```c
类型名	函数名()
{}
```

## 调用函数

#### 函数调用的形式

> 函数名(实参表列)

#### 函数调用方式

- 调用语句

- 函数表达式

  要求函数带回一个确定的值以参加表达式的运算

  ```c
  c=2*max(a,b);
  ```

- 函数参数

  函数调用作为另一个函数调用时的实参,如:

  ```c
  m=max(a,max(b,c));
  ```

### 函数调用时的数据传递

#### 形式参数和实际参数

> `形式参数`
>
> 在定义函数时函数名后面括号中的变量名称为`形式参数`

> `实际参数`
>
> 在主调函数调用一个函数时,函数名后面括号中的变量名称为`实际参数`

#### 实参和形参间的数据传递

> 在调用函数过程中,系统会把实参中的值传递给被调函数的形参.

##### 输入两个整数,要求输出其中值较大者,要求用函数来找到最大值.

```c
#include<stdio.h>

int main()
{
    int max(int x, int y);
    int a, b, c;
    printf("please enter 2 integer numbers:");
    scanf("%d,%d", &a, &b);
    c = max(a, b);
    printf("max is %d", c);
    return 0;
}

int max(int x, int y)
{c
    int z;
    z = x > y ? x : y;
    return z;
}
```

### 函数调用的执行过程

1)在定义函数中指定的形参,在未出现函数调用的时,它们并不占内存中的存储单元,在发生函数调用时,函数max的形参才被临时分配内存单元.

2)将实参的值传递给对应形参

3)在执行max函数期间,由于形参已经有值,就可以利用形参进行有关的运算

4)通过return语句将函数值带回到主调函数.

5)调用结束,形参单元被释放. 注意:实参单元仍保留并维持原值.没有改变.如果在执行一个被调函数时,形参的值发生改变,不会改变主调函数的实参的值.如:在执行max函数时,x和y的值变为10和15,但a和b仍为2和3,这是因为形参和实参是两个不同的存储单元.

### 函数的返回值

- 函数的返回值是通过return语句中获得的.

- 函数返回值的类型是函数的类型.

- 在定义函数时指定的函数类型一般应该和return语句中的表达式类型一致.

将在max函数中定义的变量z改为float型,函数返回值与指定的函数类型不同

```c
#include<stdio.h>

int main()
{
    int max(float x, float y);
    float a, b;
    int c;
    scanf("%f,%f", &a, &b);
    c = max(a, b);
    printf("max is %d", c);
    return 0;
}

int max(float x, float y)
{
    float z;
    z = x > y ? x : y;
    return (z);
}
---------
1.5,2.6
max is 2
```

### 对被调用函数的声明和函数原型

输入两个实数,用一个函数求它们之和.

```c
#include<stdio.h>

int main()
{
    float add(float x, float y);
    float a, b, c;
    scanf("%f,%f", &a, &b);
    c = add(a, b);
    printf("sum is %d", c);
    return 0;
}

float add(float x, float y)
{
    float z;
    z = x + y;
    return (z);
}
```

#### 函数声明的两种形式

1)函数类型	函数名(参数类型1	参数名1,参数类型2	参数名2,...参数类型n	参数名n)

2)函数类型	函数名(参数类型1,参数类型2,...,参数类型n)

编译系统只关心和检查参数个数和参数类型,而不检查参数名,因为在调用函数时只要求保证实参类型与形参类型一致,而不必考虑形参名是什么.因此在函数声明中

> `函数原型`函数的首部称为函数原型

### 函数的嵌套调用

##### 输入4个整数,找出其中最大的数

```c
#include<stdio.h>

int main()
{
    int max4(int a, int b, int c, int d);
    int a, b, c, d, max;
    printf("please enter 4 integer numbers:");
    scanf("%d %d %d %d", &a, &b, &c, &d);
    max = max4(a, b, c, d);
    printf("max:%d", max);
    return 0;
}

int max2(int a, int b)
{
    if (a >= b)
        return a;
    else
        return b;
}

int max4(int a, int b, int c, int d)
{
    int max2(int, int);//函数声明
    int m;
    m = max2(a, b);
    m = max2(m, c);
    m = max2(m, d);
    return m;
}
```

改进:

```c
#include<stdio.h>

int main()
{
    int max4(int a, int b, int c, int d);
    int a, b, c, d, max;
    printf("please enter 4 integer numbers:");
    scanf("%d %d %d %d", &a, &b, &c, &d);
    max = max4(a, b, c, d);
    printf("max:%d", max);
    return 0;
}

int max2(int a, int b)
{
    return a >= b ? a : b;
}

int max4(int a, int b, int c, int d)
{
    int max2(int, int);//函数声明
    return max2(max2(max2(a, b), c), d);;
}
```

## 函数的递归调用

> 一个递归的问题可以分为"回溯"和"递推"两个阶段.

用递归的方法求n! .

> n!=1				(n=0,1)
>
> n!=n×(n-1)	 (n>1)

