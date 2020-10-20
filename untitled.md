# 8.Good-use-of-pointers

> 善于利用指针

## 指针

如果在程序中定义了一个变量，在对程序进行编译时，系统就会给这个变量分配内存单元。编译系统根据程序中定义的变量类型，分配一定长度的空间。

每一个字节都有一个编号，就是“地址”。

> 地址指向该变量单元.地址形象化地称为"指针",它能找到以它为地址的内存单元.

c语言中的地址包括位置信息\(内存编号,或称为纯地址\)和它指向的数据的类型信息,或者说它是"带类型的地址".

在程序中一般通过变量名来引用变量的值,

```c
printf("%d\n",i);
```

> 对变量的访问都是通过地址进行的.

假如有输入语句:

```c
scanf("%d",&i);
```

在执行时,把键盘输入的值送到地址为2000开始的整型存储单元中.

如果有语句:

```c
k=i+j;
```

则从2000~2003字节取出i的值\(3\),再从2004~2007字节取出j的值\(6\),将它们相加后再将其和\(9\)送到k所占用的2008~2011字节单元中.这种直接按变量名进行的访问称为"直接访问"

还可以采用另一种称为“间接访问”的方式，即将变量i的地址存放在另一变量中，然后通过该变量来找到变量i的地址。从而访问i变量。

> 为了表示将数值3送到变量中，可以有两种表达方式：
>
> 1\) 将3直接送到变量i所标识的单元中,例如"i=3;"
>
> 2\) 将3送到变量_i\_pointer_所指向的单元\(即变量i的存储单元\)，例如“_i\_pointer=3;_”,其中_\*i\_pointer_表示_i\_pointer_所指向的对象，

### **指向**

通过_i\_pointer_能知道i的地址，从而找到变量_i_的内存单元。

_i\_pointer_指向_i_

## 指针变量

**通过指针变量访问整型变量.**

```c
#include<stdio.h>

int main()
{
    int a = 100, b = 10;    //定义整型变量a,b,并初始
    int *point_1, *point_2; //定义指向整型数据的指针变量point_1,point_2
    point_1 = &a;           //把变量a的地址赋给指针变量point_1
    point_2 = &b;
    printf("a=%d,b=%d\n", a, b);
    printf("*point_1=%d,*point_2=%d\n", *point_1, *point_2);
    return 0;
}
-----
a=100,b=10
*point_1=100,*point_2=10
```

### 怎么定义指针变量

```c
类型名    * 指针变量名
```

指针变量是基本数据类型派生出来的类型,它不能离开基本类型而独立存在.

### 怎样引用指针变量

1\)给指针变量赋值

```c
p=&a;
```

> 指针变量p的值是变量a的地址,p指向a.

2\)引用指针变量指向的变量.

```c
printf("%d",*p);
```

> 以整数形式输出指针变量p所指向的变量的值,即变量a的值.

3\)引用指针变量的值

```c
printf("%d",p);
```

以八进制数形式输出指针变量p的值，如果p指向了a，就是输出了a的地址，&a.

**输入a和b两个整数，按先大后小的顺序输出。**

```c
#include<stdio.h>

int main()
{
    int a, b, *point_a, *point_b, *temp;
    printf("please enter two integer number:");
    scanf("%d,%d", &a, &b);
    point_a = &a;
    point_b = &b;
    if (a < b)
    {
        temp = point_b;
        point_b = point_a;
        point_a = temp;
    }
    printf("a=%d,b=%d\n", a, b);
    printf("max=%d,min=%d\n", *point_a, *point_b);
}
```

### 指针变量作为函数参数

**:heavy\_check\_mark: 输入a和b两个整数，按先大后小的顺序输出。**

```c
#include<stdio.h>

int main()
{
    int a, b, *point_a, *point_b;
    void swap(int *point_a, int *point_b);
    printf("please enter two integer number:");
    scanf("%d,%d", &a, &b);
    point_a = &a;
    point_b = &b;
    if (a < b)
        swap(point_a, point_b);
    printf("max=%d,min=%d\n", *point_a, *point_b);
}

void swap(int *point_a, int *point_b)
{
    int temp;
    temp = *point_a;
    *point_a = *point_b;
    *point_b = temp;
}
----------------
//如果写成:
     int *temp;
    *temp = *point_a;//此语句有错误,不能向一个未知的存储单元赋值.
    point_a = *point_b;
    point_b = *temp;
```

> 由于"**单向传递**"的值传递方式，形参值的改变不能使实参的值随之改变.
>
> 为了**使在函数中改变了的变量值能被主调函数main所用**,不能采取把改变值的变量作为参数的办法,
>
> 而应该用`指针变量`作为函数的参数,**使指针变量所指向的变量值发生变化**,在函数调用结束后,这些变量值的变化**依然保留**了下来.
>
> ------------有关值传递请看：[值传递](https://kayleh.top/pass-by-value/%20)----------------------------------------------------

**:x:不可以通过执行调用函数来改变实参指针变量的值,但是可以改变实参指针变量所指变量的值:**

```c
void swap(int *point_a, int *point_b)
{
    int *temp;
    temp = point_a;
    point_a = point_b;
    point_b = temp;
}
------
×无法改变
```

### 通过指针引用数组

> 数组元素的指针就是数组元素的地址

可以用一个指针变量指向一个数组元素.如:

```c
int a[10]={1,3,5,7,9,11,13,15,17,19};
int *p;
p=&a[0];
//也可以写成p=a;
指针变量p的值是数组a首元素即a[0]的地址.而不是数组a各元素的值.
```

#### 在引用数组元素时指针的运算.

> 指针变量p指向数组元素a\[0\],希望用p+1表示指向下一个元素a\[1\];

* 系统会根据指针变量的基类型来确定增加的字节数.

  如:

  整型类型的指针变量int \*p,

  p+1就是位移一个数组元素\(int\)所占用的字节数也就是4.

* 如果p的初值为&a\[0\],则p+i和a+i就是数组元素a\[i\]的地址.
* \*\(p+i\)或\*\(a+i\)是p+i或a+i就是数组元素,即a\[i\].

  实际上编译系统在编译时,对数组元素a\[i\]就是按\*\(a+i\)处理的.即按数组首字母的地址加上相对位移量得到要找的元素的地址,然后找出该单元中的内容.

> \[ \]实际上是变址运算符,即将a\[i\]按a+i计算地址,然后找出此地址单元中的值.

如果指针变量p1和p2都是指向同一数组元素的长度,如**执行p2-p1,结果是p2-p1的值\(两个地址之差\)除以数组元素的长度.**

通过用p2-p1就可知道它们所指向元素的相对距离.

> 两个地址不能相加,如p1+p2是无实际意义的.

### 通过指针引用数组元素

#### 引用数组元素有两种方法

* 下标法\( a\[i\] \)
* 指针法\( \*\(a+i\)或\*\(p+i\),其中a是数组名,p是指向数组元素的指针变量.其初值为p=a; \)

**有一个整型数组a,有10个元素,要求输出数组中的全部元素.**

1\)下标法:

```c
#include<stdio.h>

int main()
{
    int i, a[10];
    printf("please enter 10 integer numbers:");
    for (i = 0; i < 10; i++)
        scanf("%d", &a[i]);
    for (i = 0; i < 10; i++)
        printf("%d", a[i]);
    printf("\n");
    return 0;
}
```

2\)通过数组名计算数组元素地址,找出元素的值

```c
#include<stdio.h>

int main()
{
    int i, a[10];
    printf("please enter 10 integer numbers:");
    for (i = 0; i < 10; i++)
        scanf("%d", &a[i]);
    for (i = 0; i < 10; i++)
        printf("%d\t", *(a + i));
    printf("\n");
    return 0;
}
```

3\)用指针变量指向数组元素

```c
#include<stdio.h>

int main()
{
    int a[10], *p;
    printf("please enter 10 integer numbers:");
    for (p = a; p < (a + 10); p++)
        scanf("%d", p);
    for (p = a; p < (a + 10); p++)
        printf("%d\t", *p);
    printf("\n");
    return 0;
}
```

**结论**

* 第1和2种方法执行效率是相同的. `在编译时,对数组元素a[i]就是按*(a+i)处理的`
* 第3种方法比第1和2种方法,用指针变量直接指向元素,不必每次都重新计算地址,像p++这样的自加操作是比较快的.这种有规律地改变地址值\(p++\)能大大提高效率.

> 多次使用指针变量时,应注意指针变量的位置,每次执行后可使指针变量重新指向数组a.

### 用数组名作函数参数

> 可以用数组名作函数的参数.

如:

```c
#include<stdio.h>

int main()
{
    void fun(int arr[], int n);
    int array[10];
    fun(array, 10);
    return 0;
}

void fun(int arr[], int n)
{
}
```

C编译时是将形参数组名作为指针变量来处理的.

```c
void fun(int arr[], int n)
等同于
void fun(int *arr, int n)
```

所以用数组名参数时,形参数组中各元素的值发生变化,实参数组元素的值也会发生变化.

**将数组a中n个整数按相反顺序存放.**

```c
#include<stdio.h>

int main()
{
    void inv(int [], int);
    int i, a[10];
    printf("please enter original array:\n");
    for (i = 0; i < 10; ++i)
        scanf("%d", &a[i]);
    inv(a, 10);
    printf("The array has been inverted:\n");
    for (i = 0; i < 10; ++i)
        printf("%d\t", a[i]);
    return 0;

}

void inv(int arr[], int n)
{
    int i, j,temp,m=(n-1)/2;
    for (i = 0; i < m; ++i)
    {
         j = n - 1 - i;
        temp = arr[j];
        arr[j] = arr[i];
        arr[i] = temp;
    }
}
```

