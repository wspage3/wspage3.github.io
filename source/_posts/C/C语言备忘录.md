---
title: C语言备忘录
categories: 
- C
tags: 
- C
---

## 一、库函数

### 1、system函数
1. 定义于头文件 <stdlib.h>
int system( const char *command );
以参数 command 调用宿主环境的命令处理器。

2. system("PAUSE")  是暂停的意思，等待用户信号；不然控制台程序会一闪即度过，你来不及看到执行结果。
在DOS中，命令不区分大小写，所以"pause"等价于"PAUSE"；

3. 程序
```C
#include <stdio.h>
#include <stdlib.h>
int main() {
    printf("Hello World.\n");
    system("pause");
    return 0;
}
```

<!--more-->

### 2、qsort函数
1. 定义于头文件 <stdlib.h>
void qsort(void *base, size_t nitems, size_t size, int ( *compar )(const void *, const void *))
base -- void指针：指向要排序的数组的第一个元素的指针。
nitems -- size_t类型：要排序的数组的元素个数。
size -- size_t类型：数组中每个元素的大小，以字节为单位。
compar -- 函数指针：用来比较两个元素的函数。

2. 程序
```C
#include <stdio.h>
#include <stdlib.h>
int myCompare(const void *a, const void *b) {
    return *(int*)a - *(int*)b;               // 类型强转运算符优先级高于取值运算符*
    // 返回值小于0，a排b左边。
    // 返回值等于0，不确定。
    // 返回值大于0，a排b右边。
}
int main() {
 
    int array[10] = {1, 2, 3, -1, -200};
 
    for (int i = 0; i <= 10 - 1; i++) {
        printf("%d ", array[i]);             // 1 2 3 -1 -200 0 0 0 0 0
    }
    printf("\n");
 
    qsort(array, 10, sizeof(int), myCompare);
 
    for (int i = 0; i <= 10 - 1; i++) {
        printf("%d ", array[i]);             // -200 -1 0 0 0 0 0 1 2 3
    }
    printf("\n");
  
    system("pause");
    return 0;
}
```

## 二、语法
### 0、数据类型（Data Type）
1. 整形：
【signed char】【1字节】【-128, 127】
【unsigned char】【1字节】【0, 255】
short int【至少2字节】
int
long int【至少4字节】
long long int

2. 浮点型：
float
double：ANSI规定至少和float一样长。
long double：ANSI规定至少和double一样长。

3. ANSI规定：所有浮点类型至少能容纳[10 ^ -37, 10 ^ 37]之间的任何值。

4. 浮点数字面值在缺省情况下都是double类型的。除非在后面跟L/l表示是long double类型；跟F/f表示是float类型；

5. 测试
```C
#include <stdio.h>
#include <stdlib.h>
int main() {
    char c;
    short s;
    int i;
    long l;
    long long ll;
    float f;
    double d;
    long double ld;
    printf("%d\n", sizeof(c));  // 1
    printf("%d\n", sizeof(s));  // 2
    printf("%d\n", sizeof(i));  // 4
    printf("%d\n", sizeof(l));  // 4
    printf("%d\n", sizeof(ll)); // 8
    printf("%d\n", sizeof(f));  // 4
    printf("%d\n", sizeof(d));  // 8
    printf("%d\n", sizeof(ld)); // 12
 
    printf("%d\n", sizeof(3.1));      // 8。默认为double。
    printf("%d\n", sizeof(3.1F));     // 4。指定为float。
    printf("%d\n", sizeof(3.1L));     // 12。指定为long double。
 
    printf("%d\n", sizeof("Haha"));  // 5。自动加'\0'
   
    system("pause");
    return 0;
}
```

### 1、const关键字
1. const关键字来限定一个变量是只读的；

2. 初始化：
形式1：声明时进行初始化；
形式2：作为形参，函数被调用时会得到实参的值；

3. 修饰基本数据类型
```C
#include <stdio.h>
#include <stdlib.h>
void show(const int x) {
    printf("%d\n", x);
    //x++;                     // error: increment of read-only parameter 'x'
}
int main() {
    const int a = 100;
    printf("%d\n", a);
    //a = 200;                 // error: assignment of read-only variable 'a'
 
    double const b = 8.0;
    printf("%f\n", b);
    //b = 8.1;                 // error: assignment of read-only variable 'b'
 
    show(12345);
    show(a);
 
    system("pause");
    return 0;
}
```

4. 修饰数组
```C
#include <stdio.h>
#include <stdlib.h>
int main() {
    const int a[5] = {1, 2, 3, 4, 5}; // 等价于：int const a[5]
    //a[0] = 100;                     // error: assignment of read-only location 'a[0]'
 
    system("pause");
    return 0;
}
```

5. 修饰指针-1：修饰*p
```C
#include <stdio.h>
#include <stdlib.h>
int main() {
    int a = 100;
    const int *p = &a;  // 等价于：int const *p；此时const修饰*p。
    printf("%d\n", *p); // 输出100
 
    int b = 500;
    p = &b;        // 合法，p指向了另一个变量b。
    printf("%d\n", *p); // 输出500
 
    //*p = 5000;     // 非法，p指向的内容是只读的，不可通过p进行修改。
  
    system("pause");
    return 0;
}
```

6. 修饰指针-2：修饰p
```C
#include <stdio.h>
#include <stdlib.h>
int main() {
    int a = 100;
    int * const p = &a; // 此时const修饰p。
    printf("%d %d\n", *p, a); // 输出100 100
 
    *p = 1000;     // 合法，p指向的内容可修改。
    printf("%d %d\n", *p, a); // 输出1000 1000
 
    int b = 500;
    //p = &b;        // 非法，p不能指向其它变量。
  
    system("pause");
    return 0;
}
```

7. 修饰指针-3：同时修饰p与*p
```C
#include <stdio.h>
#include <stdlib.h>
int main() {
    int a = 100;
    const int * const p = &a; // 此时const修饰p、*p。等价于：int const * const p;
    printf("%d %d\n", *p, a); // 输出100 100
 
    //*p = 1000;     // 非法，p指向的内容不可修改。
 
    int b = 500;
    //p = &b;        // 非法，p不能指向其它变量。
  
    system("pause");
    return 0;
}
```

8. 理解与记忆
若const出现在 * 的左边，意味着：p指向的内容是只读的；
若const出现在 * 的右边，意味着：p本身是只读的，不可变；

9. 借助以上理解，观察程序中出现的等价情况；

10. 用途-1（常用）：修饰函数形参，防止传入的参数被修改了。

11. 用途-2：修饰全局变量，防止任何部分都能对其修改。

12. 理解：const关键字让编译器帮助我们发现：变量不该被修改，但却修改了。
因为：可以通过某种不正规途径，来修改const修饰的变量。了解即可，不要这么做！

### 2、存储类型（Storage Class）
0. 变量的存储类型决定：变量何时创建；何时销毁；它的值保持多久；

1. 有三个地方可以用于存储变量：普通内存；运行时堆栈；硬件寄存器；

2. 自动变量：
在代码块内部声明的变量；
存储在堆栈中；
auto修饰，但代码块中缺省情况就是自动变量；
执行到声明语句时，创建；离开代码块时，销毁；
若多次执行代码块，每次在内存中的位置不保证相同；
若不指定初始值，则为一个随机值；

3. 静态变量：
代码块之外的变量；代码块内的自动变量，加static修饰；
存储在静态内存中；
程序运行前创建，整个执行期间都存在；
注：若在代码块内用static修饰了变量，其作用域仍然是本代码块内，只是修改了其存储类型；
若不指定初始值，则为0；

4. 寄存器变量：
代码块内的自动变量，加register修饰；
访问效率比内存中高；
提示编译器应该存储在硬件寄存器中，但编译器可能并不理睬；

### 3、switch
1. case后面必须是一个整数，或者是结果为整数的表达式，但不能包含任何变量。

2. 当和某个整型数值匹配成功后，会执行该分支以及后面所有分支的语句。
```C
#include <stdio.h>
#include <stdlib.h>
int main() {
    int a = 4;
    switch(a){
        case 1: printf("Monday\n");
        case 2: printf("Tuesday\n");
        case 3: printf("Wednesday\n");
        case 4: printf("Thursday\n"); // 执行输出
        case 5: printf("Friday\n");   // 执行输出
        case 6: printf("Saturday\n"); // 执行输出
        case 7: printf("Sunday\n");   // 执行输出
        default:printf("error\n");    // 执行输出
    }
  
    system("pause");
    return 0;
}
```

3. 由于default是最后一个分支，匹配后不会再执行其他分支，所以也可以不添加break语句。

4. default不是必须的。当没有default时，如果所有case都匹配失败，那么就什么都不执行。

### 4、struct
1. 声明-1
```C
#include <stdio.h>
#include <stdlib.h>
int main() {
    struct {
        int a;
        char c;
        double d;
    } x; // 创建了一个名为x的变量。它有三个成员；
 
    struct {
        int a;
        char c;
        double d;
    } y, *p; // 创建了一个名为y的变量、一个名为p的结构体指针变量；
 
    p = &y; // 合法；
    //p = &x; // error: cannot convert 'main()::<unnamed struct>*' to 'main()::<unnamed struct>*' in assignment
  
    system("pause");
    return 0;
}
```

2. 声明-2
```C
#include <stdio.h>
#include <stdlib.h>
int main() {
    struct Node {
        int a;
        char c;
        double d;
    }; // 仅仅场景了Node这个类型，没有创建任何变量；
 
    struct Node x;          // 创建一个名为x的变量，类型为Node
    struct Node array[10];  // 创建一个名为array的数组，类型为Node
   
    system("pause");
    return 0;
}
```

3. 声明-3
```C
#include <stdio.h>
#include <stdlib.h>
int main() {
    struct Node {
        int a;
        char c;
        double d;
    }; // 仅仅场景了Node这个类型，没有创建任何变量；
 
    typedef struct Node NODE;
 
    NODE x;          // 创建一个名为x的变量，类型为NODE
    NODE array[10];  // 创建一个名为array的数组，类型为NODE
  
    system("pause");
    return 0;
}
```

4. 自引用
不支持在结构体Node中，某成员结构体Node本身，引起无限递归；
支持某成员为该类型的指针；例如链表结点、二叉树结点等；
```C
#include <stdio.h>
#include <stdlib.h>
int main() {
    typedef struct Node {
        int a;
        char c;
        double d;
        struct Node *p; // 此处因为NODE还没出现，所以只能写成：struct Node。
    } NODE; // 仅仅场景了Node这个类型，没有创建任何变量；
 
    NODE x;          // 创建一个名为x的变量，类型为NODE
    NODE array[10];  // 创建一个名为array的数组，类型为NODE
  
    system("pause");
    return 0;
}
```

5. 字节对齐
```C
#include <stdio.h>
#include <stdlib.h>
int main() {
    struct Node {
        int a;    // 4
        char c;   // 1
        double d; // 8
    };
    printf("%d\n", sizeof(struct Node)); // 输出16：4 + 4 + 8。其中考虑了字节对齐。
  
    system("pause");
    return 0;
}
```

6. 初始化
```C
#include <stdio.h>
#include <stdlib.h>
int main() {
    struct Node {
        int a;
        char c;
        double d;
        int array[5];
    }; // 仅仅场景了Node这个类型，没有创建任何变量；
 
    typedef struct Node NODE;
 
    NODE x = {1, 'x', 3.14, {1, 3, 5}};
 
    printf("%d\n", x.a);        // 输出1
    printf("%c\n", x.c);        // 输出'x'
    printf("%f\n", x.d);        // 输出3.140000
    printf("%d-%d-%d-%d-%d\n", x.array[0], x.array[1], x.array[2], x.array[3], x.array[4]); // 输出1-3-5-0-0        
  
    system("pause");
    return 0;
}
```

7. offsetof：定义在头文件stddef.h中
```C
#include <stdio.h>
#include <stdlib.h>
#include <stddef.h>
int main() {
    struct Node {
        int a;         // 4
        char c;        // 1
        double d;      // 8
        int array[5];  // 4 * 5 = 20
    }; 
 
    typedef struct Node NODE;
 
    printf("%d\n", sizeof(NODE)); // 输出40: 4 + 4 + 8 + 24
 
    printf("%d\n", offsetof(NODE, a));     // 输出0
    printf("%d\n", offsetof(NODE, c));     // 输出4
    printf("%d\n", offsetof(NODE, d));     // 输出8
    printf("%d\n", offsetof(NODE, array)); // 输出16
  
    system("pause");
    return 0;
}
```

8. 结构体作为函数参数 vs 结构体指针作为函数参数
后者效率高：由于C语言是参数传值，即把实参的一份拷贝，再传给函数。若结构体非常大，则效率很低；
后者可以修改实参：若需要修改实参时，前者必须返回一个结构体，效率更低了。后者则返回void即可；

### 5、指针（Pointer）
1. NULL指针，一个特殊的指针变量，表示不指向任何东西；
对NULL进行解引用是非法的；

2. *的优先级高于+/-
【 *p + 1 】：p指向的内容，再加1；
【 *(p + 1) 】：p指向的内容的下一个内容；

3. 自增（自减）运算符：
【 p++ 】：产生p的一份拷贝p0作为表达式的值；然后p指向下一个内容；
【 ++p 】：p指向下一个内容；然后产生p此时的一份拷贝p1作为表达式的值；

4. ++/--的优先级高于*；
【 *p++ 】：产生p的一份拷贝p0作为表达式p++的值；然后p指向下一个内容；最后对p0进行间接访问；
【 *++p 】：p指向下一个内容；然后产生p此时的一份拷贝p1作为表达式++p的值；最后对p1进行间接访问；

5. 举例：获取字符串的长度；
```C
#include <stdio.h>
#include <stdlib.h>
#include <stddef.h>
int getLen(const char *string) {
    if (string == NULL) {
        return 0;
    }
 
    int len = 0;
    while (*string++ != '\0') {
        len++;
    }
    return len;
}
int main() {
    char empty[] = "";
    char hello[] = {'H', 'e', 'l', 'l', 'o', '\0'};
 
    printf("%d\n", sizeof(empty));   // 输出1
    printf("%d\n", sizeof(hello));   // 输出6
 
    printf("%d\n", getLen(empty));   // 输出0
    printf("%d\n", getLen(hello));   // 输出5
   
    system("pause");
    return 0;
}
```

6. 对指针进行加减运算：和指针类型有关系。表达式的结果也是指针；
形式1：指针指向数组中的元素时，进行加减操作；
形式2：也适用于malloc动态分配获得的内存上操作；

7. 指针 - 指针：结果是有符号整数；只有两个指针都指向同一个数组中的元素时，才能进行减操作；
```C
#include <stdio.h>
#include <stdlib.h>
#include <stddef.h>
int main() {
    char hello[] = {'H', 'e', 'l', 'l', 'o'};
    char world[] = {'W', 'o', 'r', 'l', 'd'};
 
    char *p1 = &hello[0]; // 等价于：hello
    char *p2 = &hello[4];
 
    char *p3 = &world[4];
 
    printf("%d\n", p1 - p2);   // 输出-4
    printf("%d\n", p2 - p1);   // 输出4
 
    printf("%d\n", p3 - p2);   // 输出-5，结果无意义。不要这样使用；
   
    system("pause");
    return 0;
}
```

8. 指针的关系运算：< 、<= 、> 、>=；
只有两个指针都指向同一个数组中的元素时，才能进行关系运算；
举例：将数组中所有数字置为x；
```C
#include <stdio.h>
#include <stdlib.h>
#include <stddef.h>
int main() {
    const int maxn = 10;
    float a[maxn] = {1, 2, 3, 4, 5, 6, 7, 8, 9};
 
    for (int i = 0; i <= maxn - 1; i++) {
        printf("%f ", a[i]);
    }
    printf("\n");
 
    float *p;
    // for (p = &a[0]; p < &a[maxn]; ) {
    //     *p++ = 3.14;
    // }
 
    // for (p = &a[0]; p <= &a[maxn - 1]; ) {
    //     *p++ = 3.14;
    // }
 
    // for (p = &a[maxn]; p > &a[0]; ) {
    //     *--p = 3.14;
    // }
 
    for (p = &a[maxn - 1]; p >= &a[0]; p--) {
        *p = 3.14;
    }
    // 这种写法有问题，在最后一次循环中，p指向数组a中第0个元素，然后p--。p指向数组a中第0个元素在内存中前面的那个内容。
    // 然后再进行一次比较，看是否还进行循环；而这次比较的表达式的值是未定义的。
    // 因为：C标准允许p和数组最后一个元素在内存中后面的那个位置比较；但不允许p和数组中第一个元素在内存中前面的那个位置比较；
 
    for (int i = 0; i <= maxn - 1; i++) {
        printf("%f ", a[i]);
    }
    printf("\n");
  
    system("pause");
    return 0;
}
```

9. 二级指针 + 指针数组举例：在若干个长度不等的字符串中判断字母target是否存在。
```C
#include <stdio.h>
#include <stdlib.h>
#include <stddef.h>
// 等价于：int isExist(char **strings, char target) 
int isExist(char *strings[], char target) {
    char *str = NULL;
    while ((str = *strings++) != NULL) {
        while (*str != '\0') {
            if (*str++ == target) {
                return 1;
            }
        }
    }
    return 0;
}
int main() {
 
    char *strings[] = {
        "aaabbb",
        "cccddd",
        "eee",
        "fff",
        "ggg",
        NULL
    };
    // 数组名代表：指向数组第一个元素的指针，指针的指向不许变。
    // 此处strings代表：指向strings[0]的指针，指向不许变。而strings[0]是一个char*指针。所以strings是一个char**指针；
 
    printf("%d\n", sizeof('a'));           // 输出1
    printf("%d\n", sizeof(strings));       // 输出24：指针数组有6个元素，每个元素都是一个char*指针。
    printf("%d\n", sizeof(strings[0]));    // 输出4，指针数组中的任意元素，都是一个char*指针，大小为4。
 
    for (int i = 0; i <= 4; i++) {
        printf("%s\n", strings[i]);
    }
 
    for (char c = 'a'; c <= 'z'; c++) {
        printf("%d\n", isExist(strings, c));   // 输出0/1
    }
  
    system("pause");
    return 0;
}
```

10. 二维数组 + 指向数组的指针举例：在若干个长度相等的字符串中判断字母target是否存在。
```C
#include <stdio.h>
#include <stdlib.h>
#include <stddef.h>
int isExist(char (*row)[10], int rowCnt, char target) {
    for (int i = 0; i <= rowCnt - 1; i++) {
        char *str = *(row + i);
        while (*str != '\0') {
            if (*str++ == target) {
                return 1;
            }
        }
    }
    return 0;
}
int main() {
 
    char matrix[5][10] = {
        {'a', 'b', 'c'},
        {'d', 'e', 'f'},
        {'g', 'h', 'i'},
        {'j', 'k', 'l'},
        {'m', 'n', 'o'}
    };
 
    printf("%d\n", sizeof('a'));         // 输出1
    printf("%d\n", sizeof(matrix));      // 输出50
    printf("%d\n", sizeof(matrix[0]));   // 输出10
 
    for (int i = 0; i <= 4; i++) {
        printf("%s", matrix[i]);
        printf("\n");
    }
 
    // abc
    // def
    // ghi
    // jkl
    // mno
 
    for (char c = 'a'; c <= 'z'; c++) {
        printf("%d\n", isExist(matrix, 5, c));   // 输出0/1
    }
  
    system("pause");
    return 0;
}
```

### 6、字节序
1. 对于不同主机来说，数据存储的字节序不同。x86是小端序。

2. 对于网络序，一般是大端序。

3. 小端序：低位在低地址。若int a = 0x12345678。则在小端序中，a的存储位：[78][56][34][12]。
大端序：低位在高地址。若int a = 0x12345678。则在大端序中，a的存储位：[12][34][56][78]。符合人类阅读习惯。

4. C语言提供了一组接口用于整型数据在本地序和网络序之间的转换。

5. 数据从本地传输到网络，需要转换为网络序，接收到的网络数据需要转换为本地序后使用。


### 7、offsetof
1. 定义在头文件stddef.h中。

2. 实现
```C
#define offsetof(TYPE, MEMBER) ((int)&((TYPE*)0) -> MEMBER)
```

3. 举个例子
```C
#include <stdio.h>
#include <stdlib.h>
#include <stddef.h>
#define STU_NAME_LEN 16
typedef struct student {
    int id;
    char sex;
    char name[STU_NAME_LEN];
} Student;
int main() {
 
    Student s = {14, 'M', "Tom"};
 
    printf("%d\n", sizeof(Student)); // 24
 
    printf("%d\n", &s);         // 6422176
    printf("%d\n", &s.id);      // 6422176
    printf("%d\n", &s.sex);     // 6422180
    printf("%d\n", &s.name);    // 6422181
 
    int off1 = (int)&((Student*)0) -> id;             // 成员选择->优先级高于取地址&
    int off2 = (int)&((Student*)0) -> sex;            // 成员选择->优先级高于取地址&
    int off3 = (int)&((Student*)0) -> name;           // 成员选择->优先级高于取地址&
 
    printf("%d %d %d\n", off1, off2, off3);        // 0 4 5
 
    int off4 = offsetof(Student, id);       
    int off5 = offsetof(Student, sex);        
    int off6 = offsetof(Student, name);           
 
    printf("%d %d %d\n", off4, off5, off6);        // 0 4 5
 
    char *pSex = &s.sex;
    Student *p = (Student*)((char*)pSex - off5);
 
    printf("%d-%d %c %s\n", p, p->id, p->sex, p->name);
     
    system("pause");
    return 0;
}
```

### 8、container_of
1. 定义在头文件stddef.h中。


