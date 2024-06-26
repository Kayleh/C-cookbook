# 9.users-create-data-types-themselves

## 用户自己建立数据类型

#### 定义和使用结构体变量

由不同类型数据组成的组合型的数据结构，它称为**结构体**.

```c
struct Student
    {
        int num;
        char name[20];
        char sex;
        int age;
        float score;
        char addr[30];
    };
```

声明结构体类型的一般形式

```c
struct 结构体名
    {成员表列};

结构体成员:
类型名 成员名;
```

成员可以属于另一个结构体类型

```c
struct Date{
    int month;
    int day;
    int year;
}
struct Student
{
    struct Date birthday; //成员birthday属于struct Date类型
}
```

#### 定义结构体类型变量

**先声明结构体类型,再定义该类型的变量.**

```c
struct Student student1,student2;
```

**在声明类型的同时定义变量**

```c
    struct Student
    {
        int num;
        char name[20];
        char sex;
        int age;
        float score;
        char addr[30];
    } student1, student2;
```

```c
struct 结构体名
{
    成员表列
}变量名表列;
```

**不指定类型名而直接定义结构体类型变量**

```c
struct 
{
    成员表列
}变量名表列;
```

没有名字.显然不能再以此结构类型去定义其他变量.

> * 对类型是不分配空间的,只对变量分配空间
> * 结构体类型中的成员名可以与程序中的变量名相同,但二者不代表同一对象

#### 结构体变量的初始化和引用

**把一个学生的信息(包括学号,姓名等)放在一个结构体变量中,然后输出这个学生的信息.**

```c
    struct Student
    {
        int num;
        char name[20];
        char sex;
        char addr[30];
    } a = {10101, "Li Lin", 'M', "123 Beijing Road"};
    printf("No.:%ld\nname:%s\nsex:%c\naddress:%s\n", a.num, a.name, a.sex, a.addr);
    return 0;
```

> C99标准允许对某一成员b.name.其他未被指定初始化的数值型成员被系统初始化为0.字符型成员被系统初始化为'\0',指针型成员被系统初始化为NULL

引用结构体变量中成员的值,引用方式为

```c
结构体变量名.成员名
```

`.`是成员运算符,在所有的运算符中优先级最高.

* 可以对变量的成员赋值.
*   同类的结构体变量可以互相赋值

    ```c
    student1=student2;
    ```

**输入两个学生的学号,姓名和成绩较高的学生的学号,姓名和成绩.**

```c
#include <stdio.h>

int main()
{
    struct Student
    {
        int num;
        char name[20];
        float score;
    } student1, student2;
    printf("please enter student1 num,name,score:\n");
    scanf("%d%s%f", &student1.num, student1.name, &student1.score);
    printf("please enter student2 num,name,score:\n");
    scanf("%d%s%f", &student2.num, student2.name, &student2.score);

    if (student1.score > student2.score)
        printf("No.:%ld\nname:%s\nscore:%c\n", student1.num, student1.name, student1.score);
    else if (student1.score < student2.score)
        printf("No.:%ld\nname:%s\nscore:%c\n", student2.num, student2.name, student2.score);
    else
    {
        printf("No.:%ld\nname:%s\nscore:%c\n", student1.num, student1.name, student1.score);
        printf("No.:%ld\nname:%s\nscore:%c\n", student2.num, student2.name, student2.score);
    }
    return 0;
}
```

### 使用结构体数组

**定义结构体数组**

```c
#include <stdio.h>
#include <string.h>

struct Person
{
    char name[20];
    int count;
} leader[3] = {"Li", 0, "Zhang", 0, "Sun", 0};

int main()
{
    int i, j;
    for (i = 0; i < 10; i++)
    {
        char leader_name[20];
        scanf("%s", leader_name);
        for (j = 0; j < 3; j++)
            if (strcmp(leader_name, leader[j].name) == 0)
                leader[j].count++;
    }
    printf("\nResult:\n");
    for (i = 0; i < 3; i++)
        printf("%5s:%d\n", leader[i].name, leader[i].count);
    return 0;
}
```

**定义结构体数组一般形式**

①

```c
struct 结构体名
{成员表列} 数组名[数组长度];
```

②先声明一个结构体类型(如struct Person),然后再用此类型定义结构体数组

```c
结构体类型 数组名[数组长度]
如:
struct Person leader;
```

对结构体数组初始化的形式是在定义数组的后面加上

```c
={初值表列};
```

如:

```c
struct Person  leader[3] = {"Li", 0, "Zhang", 0, "Sun", 0};
```

**有n个学生的信息(包括学号,姓名,成绩),要求按照成绩的高低顺序输出各学生的信息.**

```c
#include <stdio.h>
#include <string.h>

struct Student
{
    int num;
    char name[20];
    float score;
};

// 有n个学生的信息(包括学号,姓名,成绩),要求按照成绩的高低顺序输出各学生的信息.
int main()
{
    struct Student stu[5] = {{10101, "Zhang", 78}, {10103, "Wang", 98.5}, {10106, "Li", 86}, {10108, "Ling", 73.5}, {10110, "Sun", 100}};
    int i, j, k;
    struct Student temp;
    const int n = 5;
    for (i = 0; i < n - 1; i++)
    {
        k = i;
        for (j = i + 1; j < n; j++)
            if (stu[j].score > stu[k].score)
                k = j;
        temp = stu[k];
        stu[k] = stu[i];
        stu[i] = temp;
    }
    for (i = 0; i < n; i++)
        printf("%6d%8s%6.2f\n", stu[i].num, stu[i].name, stu[i].score);
    printf("\n");
    return 0;
}
```

### 结构体指针

#### 指向结构体变量的指针

指向结构体对象的指针变量既可指向结构体变量,也可指向结构体数组中的元素.指针变量的基类型必须与结构体变量的类型相同.如:

```c
struct Student *pt; //pt可以指向struct Student类型的变量或数组元素.
```

**通过指向结构体变量的指针变量输出结构体变量中成员的信息**

```c
#include <stdio.h>
#include <string.h>

int main()
{
    struct Student
    {
        long num;
        char name[20];
        char sex;
        float score;
    };
    struct Student stu_1;
    struct Student *p;
    p = &stu_1;
    stu_1.num = 10101;
    strcpy(stu_1.name, "Li Lin"); 
    //stu_1.name = "Li Lin"; 非法,表达式必须是可修改的左值,stu_1.name是临时的值
    stu_1.sex = 'M';
    stu_1.score = 89.5;
    printf("No.:%ld\nname:%s\nsex:%c\nscore:%5.1f\n", stu_1.num, stu_1.name, stu_1.sex, stu_1.score);
    printf("No.:%ld\nname:%s\nsex:%c\nscore:%5.1f\n", (*p).num, (*p).name, (*p).sex, (*p).score);
    return 0;
}
```

**如果p指向一个结构体变量stu,以下3种用法等价.**

* stu.成员名( 如stu.num )
* (_p).成员名( 如(\\_p).num )
* p->成员名( 如p->num )

#### 指向结构体数组的指针

> 可以用指针变量指向结构体数组的元素

**有3个同学的信息,放在结构体数组中,要求输出全部同学的信息.**

```c
#include <stdio.h>
#include <string.h>

struct Student
{
    long num;
    char name[20];
    char sex;
    int age;
};
struct Student stu[3] = {{10101, "Zhang", 'F', 1}, {10103, "Wang", 'M', 98}, {10106, "Li", 'F', 86}};

int main()
{
    struct Student *p;
    printf("No.    Name               sex   age\n");
    for (p = stu; p < stu + 3; p++) //执行p++后p的值等于stu+1,p指向stu[1]a
        printf("%5d %-20s %2c %4d\n", p->num, p->name, p->sex, p->age);
    return 0;
}
```

#### 用结构体变量和结构体变量的指针作函数参数

将一个结构体变量的值传递给另一个函数，有3种方法：

* 用结构体变量的成员作参数.
* 用结构体变量作实参
* 用指向结构体变量(或数组元素)的指针作实参,将结构体变量(或数组元素)的地址传给形参

**有n个结构体变量,内含学生学号,姓名和3门课程的成绩.要求输出平均成绩最高的学生的成绩(包括学号,姓名,3门课程绩和平均成绩).**

```c
#include <stdio.h>
#define N 3

struct Student
{
    long num;
    char name[20];
    float score[3];
    int aver;
};
// 有n个结构体变量,内含学生学号,姓名和3门课程的成绩.要求输出平均成绩最高的学生的成绩(包括学号,姓名,3门课程成绩和平均成绩).
int main()
{
    void input(struct Student stu[]);
    struct Student max(struct Student stu[]);
    void print(struct Student stud);
    struct Student stu[N], *p = stu;
    input(p);
    print(max(p));
    return 0;
}
void input(struct Student stu[])
{
    int i;
    printf("请输出各学生的信息:学号,姓名,3门课成绩:\n");
    for (i = 0; i < N; i++)
    {
        scanf("%d %s %f %f %f", &stu[i].num, stu[i].name, &stu[i].score[0], &stu[i].score[1], &stu[i].score[2]);
        stu[i].aver = (stu[i].score[0], stu[i].score[1], stu[i].score[2]) / 3.0;
    }
}
struct Student max(struct Student stu[])
{
    int i, max = 0;
    for (i = 0; i < N; i++)
    {
        if (stu[i].aver > stu[max].aver)
            max = i;
    }
    return stu[max];
}
void print(struct Student stud)
{
    printf("\n成绩最高的学生是:\n");
    printf("学生:%d\n姓名:%s\n三门课成绩:%5.1f,%5.1f,%5.1f\n平均成绩:%6.2f\n", stud.num, stud.name, stud.score[0], stud.score[1], stud.score[2]);
}
```

### 用指针处理链表

> 链表中各元素在内存中的地址可以是不连续的，要找到某一元素，必须先找到上一个元素，根据它提供的下一个元素地址才能找到一个元素.

**建立简单的静态链表**

**由3个学生数据组成的结点组成，要求输出各结点的数据.**

```c
#include <stdio.h>

struct Student
{
    long num;
    float score;
    struct Student *next;
};
int main()
{
    struct Student a, b, c, *head, *p;
    a.num = 10101;
    a.score = 89.5;
    b.num = 10103;
    b.score = 90;
    c.num = 10107;
    c.score = 85;
    head = &a;
    a.next = &b;
    b.next = &c;
    c.next = NULL;
    p = head;
    do
    {
        printf("%ld %5.1f\n", p->num, p->score);
        p = p->next;
    } while (p != NULL);

    return 0;
}
```

**建立动态链表**

**写一个函数建立一个有3名学生数据的单向动态链表**

```c
#include <stdio.h>
#include <stdlib.h>
#define LEN sizeof(struct Student)
struct Student
{
    long num;
    float score;
    struct Student *next;
};
int n;
struct Student *create(void) //此函数返回一个指向链表头的指针
{
    struct Student *head;
    struct Student *p1, *p2;
    n = 0; //结点的个数
    p1 = p2 = (struct Student *)malloc(LEN);
    scanf("%ld,%f", &p1->num, &p1->score);
    head = NULL;
    while (p1->num != 0)
    {
        n = n + 1;
        if (n == 1)
            head = p1;
        else
            p2->next = p1;//将新结点的地址赋给第1个结点的next成员
        p2 = p1; //使p2指向刚才建立的结点。
        p1 = (struct Student *)malloc(LEN);//开辟结点并使p1指向它
        scanf("%ld,%f", &p1->num, &p1->score);
    }
    p2->next = NULL;
    return (head);
}

int main()
{
    struct Student *pt;
    pt = create();
    printf("\nnum:%ld\nsocre:%5.1f\n", pt->num, pt->score); //输入0,0表示结束
    return 0;
}
```

**输出链表**

**将链表中各结点的数据依次输出print**

```c
void print(struct Student *head)
{
    struct Student *p;
    printf("\nNow,These %d records are:\n", n);
    if (head != NULL)
    {
        do
        {
            printf("\nnum:%ld\nsocre:%5.1f\n", p->num, p->score);
            p = p->next;
        } while (p != NULL);
    }
}
```

### 共用体类型

定义:

```c
union 共用体名
{
    成员表列
}变量表列;
```

如

```c
    union Data
    {
        int i;
        char ch;
        float f;
    } a, b, c;
```

结构体和共用体的区别:

* 结构体变量所占用内存长度是各成员占的内存长度之和.每个成员分别占有其自己的内存单元.
* 共用体变量所占的内存长度等于最长的成员的长度.

**引用共用体变量的方式**

```c
a.ch;
a.i;
```

**共用体类型数据的特点**

* 同一内存段可以用来存放几种不同类型的成员,但在每一瞬间时只能存放其中一个成员,而不是同时存放几个.因为在每一瞬时,存储单元只能有唯一的内容,也就是说,在共用体变量中只能存放一个值.
*   可以对共用体变量初始化,但初始化表中只能有一个常量.

    ```c
    #include <stdio.h>
    int main()
    {
        union Data
        {
            int i;
            char ch;
            float f;
        } a = {1, 'a', 1.5}; //非法，不能初始化3个成员，它们占用同一段存储单元.
        union Data a = {16};
        union Data a = {.ch='j'};
    }
    ```
* 共用体变量中起作用的成员是最后一次被赋值的成员，在对共用体变量中的一个成员赋值，原有变量存储单元中的值就取代。
*   共用体变量的地址和它的各成员的地址都是同一地址。例如：

    ```c
    &a.i,&a.c,&a.f都是同一值.
    ```
*   不能对共用体变量赋值,也不能企图引用变量名来得到一个值.

    ```c
    a=1;//非法
    int m=a;//非法
    C99允许同类型的共用体变量互相赋值.如:
    b=a;//a和b是同一类型的共同体变量
    ```
* C99允许用共同体变量作函数参数.
*   共同体类型可以出现在结构体类型定义中,也可以定义共用体数组.

    结构体也可以出现在共用体类型定义中,数组也可以作为共用体的成员

**有若干个人员的数据,其中有学生和教师.学生的数据中包括:姓名,号码,性别,职业,班级.教师的数据包括:姓名,号码,性别,职业,职务.要求用同一个表格来处理.**

```c
#include <stdio.h>

struct
{
    int num;
    char name[10];
    char sex;
    char job;
    union
    {
        int clas;          //班级
        char position[10]; //职务
    } category;            //共用体变量
} person[2];               //结构体数组
int main()
{
    int i;
    for (i = 0; i < 2; i++)
    {
        printf("please enter the data of person:\n");
        scanf("%d %s %c %c", &person[i].num, &person[i].name, &person[i].sex, &person[i].job);
        if (person[i].job == 's')
            scanf("%d", &person[i].category.clas); //如是学生输入班级
        else if (person[i].job == 't')
            scanf("%d", &person[i].category.position); //如是老师输入职务
        else
            printf("Input error!");
    }
    printf("\n");
    printf("No.     name        sex     job class/position\n");
    for (i = 0; i < 2; i++)
    {
        if (person[i].job == 's')
            printf("%-6d%-10s%-4c%-4c%-10d\n", person[i].num, person[i].name, person[i].sex, person[i].job, person[i].category.clas);
        else
            printf("%-6d%-10s%-4c%-4c%-10d\n", person[i].num, person[i].name, person[i].sex, person[i].job, person[i].category.position);
    }
    return 0;
}
```

### 使用枚举类型

如果一个变量只有几种可能的值,则可以定义为**枚举(enumeration)类型**,所谓"枚举"就是把可能的值一一列举出来,变量的值只限于列举出来的值的范围内.

**定义**

如:

```c
    enum Weekday
    {
        sun,
        mon,
        tue,
        wed,
        thu,
        fri,
        sat
    };
```

用此类型来定义变量

```c
    enum Weekday workday, weekend;
```

workday, weekend被定义为**枚举变量**，花括号中的 sun,mon,tue,wed,thu,fri,sat称为**枚举元素**或**枚举常量**。

声明枚举类型的一般形式为：

```c
enum [枚举名]{枚举元素列表};
```

**说明**

* C编译器对枚举类型的枚举元素按常量处理
*   每个枚举元素都代表一个整数,按定义时的顺序,默认为0,1,2,3....

    也可以人为地指定枚举元素的数值

    ```c
    enum Weekday
        {
            sun=7,
            mon=1,
            tue,
            wed,
            thu,
            fri,
            sat
        };
    ```

    以后的顺序加1,sat为6.
*   枚举元素可以用来作判断比较.

    ```c
    if(workday==mon)...
    ```

**口袋中有红,黄,蓝,白,黑5种颜色的球若干个.每次从口袋中先后取出3个球,问得到3种不同颜色的球的可能取法,输出每种排列的情况.**

```c
#include <stdio.h>

int main()
{
    // 口袋中有红,黄,蓝,白,黑5种颜色的球若干个.每次从口袋中先后取出3个球,问得到3种不同颜色的球的可能取法,输出每种排列的情况.
    enum Color
    {
        red,
        yellow,
        blue,
        white,
        black
    };
    enum Color i, j, k, pri;
    int n, loop;
    n = 0;
    for (i = red; i <= black; i++)     //第一个球
        for (j = red; j <= black; j++) //第二个球
            if (i != j)
                for (k = red; k <= black; k++) //第三个球
                {
                    if ((k != j) && (k != i))
                    {
                        n++;
                        printf("%-4d", n);             //输出当前是第几个符合条件的组合
                        for (loop = 1; loop <= 3; i++) //先后对3个球分别处理
                        {
                            switch (loop)
                            {
                            case 1:
                                pri = i;
                                break;
                            case 2:
                                pri = j;
                            case 3:
                                pri = k;
                            default:
                                break;
                            }
                            switch (pri)
                            {
                            case red:
                                printf("%-10s", "red");
                                break;
                            case yellow:
                                printf("%-10s", "yellow");
                                break;
                            case blue:
                                printf("%-10s", "blue");
                                break;
                            case white:
                                printf("%-10s", "white");
                                break;
                            case black:
                                printf("%-10s", "black");
                                break;
                            default:
                                break;
                            }
                        }
                        printf("\n");
                    }
                }

    printf("\nTotal:%5d\n", n);
    return 0;
}
```

### 用typedef声明新类型名

简单地用一个新的类型名代替原有的类型名。

```c
    typedef int Integer;
    Integer i, j; //作用与int相同
```

命名一个简单的类型名代替复杂的类型表示方法

*   命名一个新的类型名代表结构体类型

    ```c
        typedef struct
        {
            int month;
            int day;
            int year;
        } Date;

        Date birthday;
        Date *p;
    ```
*   代表数组类型

    ```c
        typedef int Num[10];
        Num a;
    ```
*   代表指针类型

    ```c
        typedef char *String;
        String p, s[10];//p为字符指针变量,s为字符指针数组
    ```
*   代表指向函数的指针类型

    ```c
        typedef int (*Pointer)();
        Pointer p1, p2;
    ```

**typedef和#define表面上有相似之处**

```c
typedef int Count;
和
#define Count int;
```

## define是在预编译时处理的.它只能做简单的字符串替换

而typedef是在编译阶段处理的,采用如同定义变量的方法那样先生成一个类型名,然后用它去定义变量.

> 当不同源文件中用到同一类型数据,常用typedef声明一些数据类型.

使用typedef名称有利于程序的通用和移植.有的计算机系统int型数据占用不一样,需要修改:

```c
typedef int Integer;
修改为:
typedef long Integer;
```
