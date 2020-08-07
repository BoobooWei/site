---
title: 初识数组
---

# 数组基础

{% note warn 个人理解%}

C++中的数组 与 Python的列表List对比

| 对比                       | C++ Array                                                    | Python List                                                  |
| -------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 声明一维数组               | type arrayName [ arraySize ];                                | 不需要                                                       |
| 初始化                     | int balance[2] = {1000, 2};                                  | balance = [1, 'a']                                           |
| 存储的数据类型             | 必须相同                                                     | 可以不同                                                     |
| 获取长度                   | int num_ach = sizeof(balance) / sizeof(int);                 | num_ach = len(balance)                                       |
| 获取指定位置的数据         | balance[index]                                               | balance[index]                                               |
| 循环打印数组中每个元素的值 | for ( int j = 0; j < num_ach; j++ )<br/>   {<br/>      cout <<  n[ j ] << endl;<br/>   } | for i in num_ach:<br/>    print(i)<br/>for b in balance:<br/>    print(b) |
| 获取最大最小值             | 需要自定义算法比较                                           | 内置方法 max() min() <br/>此时要求列表中的元素为同一类型。   |
| 删除                       | 1. 找到要删除的元素下标记 <br/>2. 从找到的下标开始，将后面的元素依次赋值给前一位 | del balance[1]                                               |
| 插入                       | balance[num_ach+1] = 10                                      | balance.append(10)                                           |
| 声明多维数组               | type name[size1][size2]...[sizeN];                           | balance = [ [1,'tom'], [2. 'jack']]                          |

{% endnote %}

C++ 支持**数组**数据结构，它可以存储一个固定大小的相同类型元素的顺序集合。数组是用来存储一系列数据，但它往往被认为是一系列相同类型的变量。

数组的声明并不是声明一个个单独的变量，比如 number0、number1、...、number99，而是声明一个数组变量，比如 numbers，然后使用 numbers[0]、numbers[1]、...、numbers[99] 来代表一个个单独的变量。数组中的特定元素可以通过索引访问。

所有的数组都是由连续的内存位置组成。最低的地址对应第一个元素，最高的地址对应最后一个元素。

## 声明数组

在 C++ 中要声明一个数组，需要指定元素的类型和元素的数量，如下所示：

```
type arrayName [ arraySize ];
```

这叫做一维数组。**arraySize** 必须是一个大于零的整数常量，**type** 可以是任意有效的 C++ 数据类型。例如，要声明一个类型为 double 的包含 10 个元素的数组 **balance**，声明语句如下：

```
double balance[10];
```

现在 *balance* 是一个可用的数组，可以容纳 10 个类型为 double 的数字。

## 初始化数组

在 C++ 中，您可以逐个初始化数组，也可以使用一个初始化语句，如下所示：

```
double balance[5] = {1000.0, 2.0, 3.4, 7.0, 50.0};
```

大括号 { } 之间的值的数目不能大于我们在数组声明时在方括号 [ ] 中指定的元素数目。

如果您省略掉了数组的大小，数组的大小则为初始化时元素的个数。因此，如果：

```
double balance[] = {1000.0, 2.0, 3.4, 7.0, 50.0};
```

您将创建一个数组，它与前一个实例中所创建的数组是完全相同的。下面是一个为数组中某个元素赋值的实例：

```
balance[4] = 50.0;
```

上述的语句把数组中第五个元素的值赋为 50.0。所有的数组都是以 0 作为它们第一个元素的索引，也被称为基索引，数组的最后一个索引是数组的总大小减去 1。以下是上面所讨论的数组的的图形表示：

![数组表示](https://www.runoob.com/wp-content/uploads/2014/08/array_presentation.jpg)

## 访问数组元素

数组元素可以通过数组名称加索引进行访问。元素的索引是放在方括号内，跟在数组名称的后边。例如：

```
double salary = balance[9];
```

上面的语句将把数组中第 10 个元素的值赋给 salary 变量。下面的实例使用了上述的三个概念，即，声明数组、数组赋值、访问数组：

```c++
#include <iostream>
using namespace std;
 
#include <iomanip>
using std::setw;
 
int main ()
{
   int n[ 10 ]; // n 是一个包含 10 个整数的数组
 
   // 初始化数组元素          
   for ( int i = 0; i < 10; i++ )
   {
      n[ i ] = i + 100; // 设置元素 i 为 i + 100
   }
   cout << "Element" << setw( 13 ) << "Value" << endl;
 
   // 输出数组中每个元素的值                     
   for ( int j = 0; j < 10; j++ )
   {
      cout << setw( 7 )<< j << setw( 13 ) << n[ j ] << endl;
   }
 
   return 0;
}
```



上面的程序使用了 **setw()** 函数来格式化输出。当上面的代码被编译和执行时，它会产生下列结果：

```
Element        Value
      0          100
      1          101
      2          102
      3          103
      4          104
      5          105
      6          106
      7          107
      8          108
      9          109
```

# 数组操作

## 排序算法

此处不展开讲解，具体参考《数据结构与算法分析C++》

使用C++，对于最一般的内部排序应用，选用的方法不是插入排序、谢尔排序，就是快速排序，它们的选用主要是根据输入的大小来决定的。

不同的算法排序的比较（所有时间均以秒计）

| N     | 插入排序<br/>O(N^2) | 谢尔排序<br/>O(N^7/6) | 堆排序<br/>O(NlogN) | 快速排序<br/>O(NlogN) |
| ----- | ------------------- | --------------------- | ------------------- | --------------------- |
| 10    | 0.000001            | 0.000002              | 0.000003            | 0.000002              |
| 100   | 0.000106            | 0.000039              | 0.000052            | 0.000023              |
| 1000  | 0.011240            | 0.000678              | 0.000750            | 0.000316              |
| 1万   | 1.047               | 0.009782              | 0.010215            | 0.004129              |
| 10万  | 110.492             | 0.13438               | 0.139542            | 0.052790              |
| 100万 | NA                  | 1.6777                | 1.7967              | 0.6154                |

高度优化的快速排序算法即使对于很少的输入也能像谢尔排序一样快。

快速排序的改进算法仍然有O(N^2)最坏情况，但是机会是微不足道的。

如果不想过多的考虑输入是随机的大小，那么谢尔排序是不错的选择。

一般大家脑子里的只有“替换选择”算法哈——打擂台法

## 删除插入

删除：

1. 找到要删除的元素下标记
2. 从找到的下标开始，将后面的元素依次赋值给前一位

插入：

1. 将新值放在数组的末尾，重新排序

# 二维数组

在 C++ 中，数组是非常重要的，我们需要了解更多有关数组的细节。下面列出了 C++ 程序员必须清楚的一些与数组相关的重要概念：

| 概念                                                         | 描述                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [多维数组](https://www.runoob.com/cplusplus/cpp-multi-dimensional-arrays.html) | C++ 支持多维数组。多维数组最简单的形式是二维数组。           |
| [指向数组的指针](https://www.runoob.com/cplusplus/cpp-pointer-to-an-array.html) | 您可以通过指定不带索引的数组名称来生成一个指向数组中第一个元素的指针。 |
| [传递数组给函数](https://www.runoob.com/cplusplus/cpp-passing-arrays-to-functions.html) | 您可以通过指定不带索引的数组名称来给函数传递一个指向数组的指针。 |
| [从函数返回数组](https://www.runoob.com/cplusplus/cpp-return-arrays-from-function.html) | C++ 允许从函数返回数组。                                     |



C++ 支持多维数组。多维数组声明的一般形式如下：

```c++
type name[size1][size2]...[sizeN];
```

例如，下面的声明创建了一个三维 5 . 10 . 4 整型数组：

```c++
int threedim[5][10][4];
```

多维数组最简单的形式是二维数组。一个二维数组，在本质上，是一个一维数组的列表。声明一个 x 行 y 列的二维整型数组，形式如下：

```
type arrayName [ x ][ y ];
```

其中，**type** 可以是任意有效的 C++ 数据类型，**arrayName** 是一个有效的 C++ 标识符。

一个二维数组可以被认为是一个带有 x 行和 y 列的表格。下面是一个二维数组，包含 3 行和 4 列：

![C++ 中的二维数组](https://www.runoob.com/wp-content/uploads/2014/09/two_dimensional_arrays.jpg)

因此，数组中的每个元素是使用形式为 a[ i , j ] 的元素名称来标识的，其中 a 是数组名称，i 和 j 是唯一标识 a 中每个元素的下标。

## 初始化

多维数组可以通过在括号内为每行指定值来进行初始化。下面是一个带有 3 行 4 列的数组。

```
int a[3][4] = {  
 {0, 1, 2, 3} ,   /*  初始化索引号为 0 的行 */
 {4, 5, 6, 7} ,   /*  初始化索引号为 1 的行 */
 {8, 9, 10, 11}   /*  初始化索引号为 2 的行 */
};
```

内部嵌套的括号是可选的，下面的初始化与上面是等同的：

```
int a[3][4] = {0,1,2,3,4,5,6,7,8,9,10,11};
```

## 访问元素

二维数组中的元素是通过使用下标（即数组的行索引和列索引）来访问的。例如：

```
int val = a[2][3];
```

上面的语句将获取数组中第 3 行第 4 个元素。您可以通过上面的示意图来进行验证。让我们来看看下面的程序，我们将使用嵌套循环来处理二维数组：

```
#include <iostream>
using namespace std;
 
int main ()
{
   // 一个带有 5 行 2 列的数组
   int a[5][2] = { {0,0}, {1,2}, {2,4}, {3,6},{4,8}};
 
   // 输出数组中每个元素的值                      
   for ( int i = 0; i < 5; i++ )
      for ( int j = 0; j < 2; j++ )
      {
         cout << "a[" << i << "][" << j << "]: ";
         cout << a[i][j]<< endl;
      }
 
   return 0;
}
```

当上面的代码被编译和执行时，它会产生下列结果：

```
a[0][0]: 0
a[0][1]: 0
a[1][0]: 1
a[1][1]: 2
a[2][0]: 2
a[2][1]: 4
a[3][0]: 3
a[3][1]: 6
a[4][0]: 4
a[4][1]: 8
```

如上所述，您可以创建任意维度的数组，但是一般情况下，我们创建的数组是一维数组和二维数组。

# 向量容器

{% note warn 数组的替代品%}

数组的替代品:向量容器vector

{% endnote %}

## 定义和初始化

向量（Vector）是一个封装了动态大小数组的顺序容器（Sequence Container）。跟任意其它类型容器一样，它能够存放各种类型的对象。可以简单的认为，向量是一个能够存放任意类型的动态数组。

* 动态数组，可以运行阶段设置长度
* 具有数组的快速索引方式
* 可以插入和删除元素

```c++
#include <vector> 
using namespace std;

vector <double> vec1; // 未指定个数
vector <string> vec2(5); // 5个元素，每个元素的数据类型都是字符串
vector <int> vec3(20,998); // 20个元素，每个元素的值都是998
```

## 容器特性

* 顺序序列：顺序容器中的元素按照严格的线性顺序排序。可以通过元素在序列中的位置访问对应的元素。

* 动态数组：支持对序列中的任意元素进行快速直接访问，甚至可以通过指针算述进行该操作。操供了在序列末尾相对快速地添加/删除元素的操作。

* 能够感知内存分配器的（Allocator-aware）：容器使用一个内存分配器对象来动态地处理它的存储需求。

## 基本函数

### 构造函数

- vector():创建一个空vector
- vector(int nSize):创建一个vector,元素个数为nSize
- vector(int nSize,const t& t):创建一个vector，元素个数为nSize,且值均为t
- vector(const vector&):复制构造函数
- vector(begin,end):复制[begin,end)区间内另一个数组的元素到vector中

### 增加函数

- void push_back(const T& x):向量尾部增加一个元素X
- iterator insert(iterator it,const T& x):向量中迭代器指向元素前增加一个元素x
- iterator insert(iterator it,int n,const T& x):向量中迭代器指向元素前增加n个相同的元素x
- iterator insert(iterator it,const_iterator first,const_iterator last):向量中迭代器指向元素前插入另一个相同类型向量的[first,last)间的数据

### 删除函数

- iterator erase(iterator it):删除向量中迭代器指向元素
- iterator erase(iterator first,iterator last):删除向量中[first,last)中元素
- void pop_back():删除向量中最后一个元素
- void clear():清空向量中所有元素

### 遍历函数

- reference at(int pos):返回pos位置元素的引用
- reference front():返回首元素的引用
- reference back():返回尾元素的引用
- iterator begin():返回向量头指针，指向第一个元素
- iterator end():返回向量尾指针，指向向量最后一个元素的下一个位置
- reverse_iterator rbegin():反向迭代器，指向最后一个元素
- reverse_iterator rend():反向迭代器，指向第一个元素之前的位置

### 判断函数

- bool empty() const:判断向量是否为空，若为空，则向量中无元素

### 大小函数

- int size() const:返回向量中元素的个数
- int capacity() const:返回当前向量所能容纳的最大元素值
- int max_size() const:返回最大可允许的vector元素数量值

### 其他函数

- void swap(vector&):交换两个同类型向量的数据
- void assign(int n,const T& x):设置向量中第n个元素的值为x
- void assign(const_iterator first,const_iterator last):向量中[first,last)中元素设置成当前向量元素

### 看着清楚

> 1.push_back 在数组的最后添加一个数据
>
> 2.pop_back 去掉数组的最后一个数据
>
> 3.at 得到编号位置的数据
>
> 4.begin 得到数组头的指针
>
> 5.end 得到数组的最后一个单元+1的指针
>
> 6.front 得到数组头的引用
>
> 7.back 得到数组的最后一个单元的引用
>
> 8.max_size 得到vector最大可以是多大
>
> 9.capacity 当前vector分配的大小
>
> 10.size 当前使用数据的大小
>
> 11.resize 改变当前使用数据的大小，如果它比当前使用的大，者填充默认值
>
> 12.reserve 改变当前vecotr所分配空间的大小
>
> 13.erase 删除指针指向的数据项
>
> 14.clear 清空当前的vector
>
> 15.rbegin 将vector反转后的开始指针返回(其实就是原来的end-1)
>
> 16.rend 将vector反转构的结束指针返回(其实就是原来的begin-1)
>
> 17.empty 判断vector是否为空
>
> 18.swap 与另一个vector交换数据

------



# 课堂练习

## 数组基本操作

* 使用一维数组存放学生单门学科的成绩
* 求最好成绩和最差成绩
* 打印奇数个数 和 偶数个数
* 查找输入的数字在数组中的下表，没有找到的则下标-1

```c++
#include <iostream>
using namespace std;

#include <iomanip>
using std::setw;

int main()
{
    // 使用一维数组存放学生单门学科的成绩
    int achievement[11] = {77, 34, 10, 100, 53, 25, 99, 44, 76, 69, 78};

    // 计算数组的长度
    int num_ach = sizeof(achievement) / sizeof(int);

    // 累加操作
    int sum = 0;
    for (int i = 0; i < num_ach; i++)
    {
        sum = sum + achievement[i];
    }

    // 求最好成绩和最差成绩
    int max_ach = achievement[0];
    int min_ach = achievement[0];
    for (int i = 0; i < num_ach; i++)
    {
        if (achievement[i] > max_ach)
        {
            max_ach = achievement[i];
        }

        if (achievement[i] < min_ach)
        {
            min_ach = achievement[i];
        }
    }
    cout << "MAX" << setw(13) << max_ach
         << "\n"
         << "MIN" << setw(13) << min_ach << endl;

    // 打印奇数个数 和 偶数个数
    // 10/2=5
    // 11/2=5..1 偶数 6 5
    int a = num_ach / 2;
    int b = num_ach % 2;
    int odd_number = 0;  // 奇数个数
    int even_number = 0; // 偶数个数
    if (b == 0)
    {
        odd_number = a;
        even_number = a;
    }
    else if (b == 1)
    {
        odd_number = a;
        even_number = a + b;
    }
    else
    {
        cerr << "Error message : "
             << "计算异常" << endl;
    }

    cout << setw(7) << "奇数个数" << setw(13) << odd_number << endl;
    cout << setw(7) << "偶数个数" << setw(13) << even_number << endl;

    // 查找输入的数字在数组中的下标，没有找到的则下标-1
    int in_num;
    int k;
    cout << "输入待查找的数组下标：" << endl;
    cin >> in_num;
    if (in_num > -1 && in_num <= num_ach)
    {
        k = in_num;
    }
    else
    {
        k = -1;
    }
    cout << setw(7) << "查找[" << k << "]" << setw(13) << achievement[k] << endl;

    return 0;
}
```

运行结果：

```bash
MAX          100
MIN           10
奇数个数            5
偶数个数            6
输入待查找的数组下标：
2
查找[2]           10
```

## 元素删除与插入

要求：根据要求操作排行榜战斗力值

1. 战力值从大到小排序
2. 删除200
3. 插入战斗力41000并保持降序

```c++
#include <iostream>
using namespace std;

#include <iomanip>
using std::setw;

int main()
{
    // 使用维数组存放战斗力
    int hero[]{
        100,
        200,
        300,
        400};
    const int ROW = sizeof(hero) / sizeof(int);
    cout << "=====当前战斗力明细:=====" << endl;
    for (int i = 0; i < ROW; i++)
    {
        cout << "战斗力：" << hero[i] << endl;
    }
    // 删除战斗力200
    // 1. 找到200的索引位
    // 2. 将后一位覆盖前一位
    cout << "=====请输入待删除的战斗力: ";
    int data_del;
    int index_del;
    cin >> data_del;
    for (int i = 0; i < ROW; i++)
    {
        if (hero[i] == data_del)
        {
            index_del = i;
        }
    }
    cout << "@待删除的元素索引位: " << index_del << endl;



    for (int i = index_del; i < ROW - 1; i++)
    {
        hero[i] = hero[i + 1];
    }

    cout << "=====删除后:=====" << endl;
    for (int i = 0; i < ROW - 1; i++)
    {
        cout << "战斗力：" << hero[i] << endl;
    }

    // 插入41000的战斗力
    // 1. 插入到末尾
    // 2. 重新排序[先不做]
    int data_insert;
    cout << "=====请输入待插入的战斗力: ";
    cin >> data_insert;
    cout << "=====待插入战斗力：" << data_insert << endl;
    hero[ROW-1] = data_insert;
    cout << "=====插入后:=====" << endl;
    cout << "=====ROW:=====" << ROW << endl;
    for (int i = 0; i < ROW; i++)
    {
        cout << "战斗力：" << hero[i] << endl;
    }

    return 0;
}
```

运行结果：

```bash
=====当前战斗力明细:=====
战斗力：100
战斗力：200
战斗力：300
战斗力：400
=====请输入待删除的战斗力: 200
@待删除的元素索引位: 1
=====删除后:=====
战斗力：100
战斗力：300
战斗力：400
=====请输入待插入的战斗力: 900
=====待插入战斗力：900
=====插入后:=====
=====ROW:=====4
战斗力：100
战斗力：300
战斗力：400
战斗力：900
```

## 二维数组

要求：

1. 记录学生的多门学科成绩

```c++
#include <iostream>
using namespace std;

#include <iomanip>
using std::setw;

int main()
{
    // 使用二维数组存放学生多门学科的成绩
    string stu_names[]{"superman", "batman", "wonderwoman", "booboo"};
    string cource_names[]{"english", "math", "yuwen"};
    const int ROW = sizeof(stu_names) / sizeof(stu_names[0]);
    const int COL = sizeof(cource_names) / sizeof(cource_names[0]);
    cout << ROW << '\t' << COL << endl;

    double scores[ROW][COL];
    // 录入数据
    for (int i = 0; i < ROW; i++)
    {
        for (int j = 0; j < COL; j++)
        {
            cout << stu_names[i] << "'s" << cource_names[j] << " score: ";
            cin >> scores[i][j];
        }
    }
    //  打印数据
    cout << '\t';
    for (int i = 0; i < COL; i++)
    {
        cout << cource_names[i] << "\t";
    }
    cout << "\n"
         << endl;
    for (int i = 0; i < ROW; i++)
    {
        cout << stu_names[i] << "\t";
        for (int j = 0; j < COL; j++)
        {
            cout << scores[i][j] << "\t";
        }
        cout << "\n"
             << endl;
    }

    return 0;
}
```

运行结果：

```bash
4       3
superman'senglish score: 2
superman'smath score: 2
superman'syuwen score: 2
batman'senglish score: 2
batman'smath score: 2
batman'syuwen score: 2
wonderwoman'senglish score: 2
wonderwoman'smath score: 2
wonderwoman'syuwen score: 2
booboo'senglish score: 2
booboo'smath score: 2
booboo'syuwen score: 2
        english math    yuwen

superman        2       2       2

batman  2       2       2

wonderwoman     2       2       2

booboo  2       2       2
```

## 向量容器练习

```c++
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

#include <iomanip>
using std::setw;

int main()
{
    /* C++ vector VS Python List
    *  追加元素 push_back()                             append() 
    *  数组长度 size()                                  __len__()
    *  迭代器   vector<double>::iterator it;            __iter__()
    *          for (it = vec_double.begin(); it != vec_double.end(); it++){ } 
    *  排序     
    */
    vector<double> vec_double = {98.5, 67.9, 55.8, 100.1, 13.2};
    // 遍历
    cout << "初始化数组长度：" << vec_double.size() << endl;
    cout << "初始化数组元素：" << endl;
    for (int i = 0; i < vec_double.size(); i++)
    {
        cout << vec_double[i] << endl;
    }
    // 向数组中插入数字
    cout << "向数组中插入数字：77.7" << endl;
    vec_double.push_back(77.7); //末尾追加一个元素 类似python的append()方法
    cout << "插入后数组长度：" << vec_double.size() << endl;
    cout << "插入后数组元素：" << endl;
    // 遍历
    for (int i = 0; i < vec_double.size(); i++)
    {
        cout << vec_double[i] << endl;
    } // 获取数字的大小 size()方法 类似python list的__len__() 方法

    // 集合的通用遍历，使用迭代器 iterator
    // 以下是迭代器的基本用法，高能慎重
    vector<double>::iterator it; // 得到迭代器对象
    // 排序
    cout << "正序排列：" << endl;
    sort(vec_double.begin(), vec_double.end());
    // it.begin
    for (it = vec_double.begin(); it != vec_double.end(); it++)
    {
        cout << *it << endl;
    }
    cout << "倒序排列：" << endl;
    reverse(vec_double.begin(), vec_double.end());
    // it.begin
    for (it = vec_double.begin(); it != vec_double.end(); it++)
    {
        cout << *it << endl;
    }
    // 类似 Python list的 __iter__() 方法

    return 0;
}
```

运行结果：

```bash
初始化数组长度：5     
初始化数组元素：      
98.5
67.9
55.8
100.1
13.2
向数组中插入数字：77.7
插入后数组长度：6     
插入后数组元素：
98.5
67.9
55.8
100.1
13.2
77.7
正序排列：
13.2
55.8
67.9
77.7
98.5
100.1
倒序排列：
100.1
98.5
77.7
67.9
55.8
13.2
```







{% note info 学习感受%}

我已经学习过的语言包括：

大学（汇编语言、C语言）

工作（Bash Shell、SQL、PL/SQL、Python）

现在（C++）

对比下来，Python确实简单，快，美，不过我也很期待C++高级的功能，迫不及待想学习更多哈

数组的替代品 向量容器 vector 类似Python的 List。

{% endnote %}