---
title: 函数
---

{% note warn 小结%}

1.函数是有名字的计算单元，对程序(就算是小程序)的结构化至关重要。

2.函数的定义由`返回类型`、`函数名`、`形参表`(可能为空)以及`函数体`组成。

* 函数体是 调用函数时执行的语句块。
* 在调用函数时，传递给函数的实参必须与相应的形参类型兼容。
* 给函数传递实参遵循变量初始化的规则。非引用类型的形参以相应实参的副本初始化。对(非引用)形参的任何修改仅作用于局部副本，并不影响实参本身。
* 复制庞大而复杂的值有昂贵的开销。为了避免传递副本的开销，可将形参指定为引用类型。对引用形参的任何修改会直接影响实参本身。应将不需要修改相应实参的引用形参定义为 `const` 引用。
* 在 C++ 中，函数可以重载。只要函数中形参的个数或类型不同，则同一个函数名可用于定义不同的函数。编译器将根据函数调用时的实参确定调用哪一个函数。在重载函数集合中选择适合的函数的过程称为函数匹配。

3.C++ 提供了两种特殊的函数:`内联函数`和`成员函数`。

* 将函数指定为内联是建议编译器在调用点直接把函数代码展开。
* 内联函数避免了调用函数的代价。
* 成员函数则是身为类成员的函数。

4.自定义函数

```c++
int sum(int, int); //函数原型
int main()
{
    // 函数调用
    int result = sum(5, 3);
}
// 函数定义
int sum(int num1, int num2)
{
    // 函数实现的代码
}
```
{% endnote %}

# 函数是什么

函数是一组一起执行一个任务的语句。每个 C++ 程序都至少有一个函数，即主函数 **main()** ，所有简单的程序都可以定义其他额外的函数。

您可以把代码划分到不同的函数中。如何划分代码到不同的函数中是由您来决定的，但在逻辑上，划分通常是根据每个函数执行一个特定的任务来进行的。

函数**声明**告诉编译器函数的名称、返回类型和参数。函数**定义**提供了函数的实际主体。

C++ 标准库提供了大量的程序可以调用的内置函数。例如，函数 **strcat()** 用来连接两个字符串，函数 **memcpy()** 用来复制内存到另一个位置。

函数还有很多叫法，比如方法、子例程或程序，等等。

## 定义函数

C++ 中的函数定义的一般形式如下：

```c++
return_type function_name( parameter list ) 
{   
  body of the function 
}
```

在 C++ 中，函数由一个函数头和一个函数主体组成。下面列出一个函数的所有组成部分：

- **返回类型：**一个函数可以返回一个值。**return_type** 是函数返回的值的数据类型。有些函数执行所需的操作而不返回值，在这种情况下，return_type 是关键字 **void**。
- **函数名称：**这是函数的实际名称。函数名和参数列表一起构成了函数签名。
- **参数：**参数就像是占位符。当函数被调用时，您向参数传递一个值，这个值被称为实际参数。参数列表包括函数参数的类型、顺序、数量。参数是可选的，也就是说，函数可能不包含参数。
- **函数主体：**函数主体包含一组定义函数执行任务的语句。



**max()** 函数的源代码。该函数有两个参数 num1 和 num2，会返回这两个数中较大的那个数

```c++
// 函数返回两个数中较大的那个数
 
int max(int num1, int num2) 
{
   // 局部变量声明
   int result;
 
   if (num1 > num2)
      result = num1;
   else
      result = num2;
 
   return result; 
}
```



## 函数声明

函数**声明**会告诉编译器函数名称及如何调用函数。函数的实际主体可以单独定义。

函数声明包括以下几个部分：

```c++
return_type function_name( parameter list );
```

针对上面定义的函数 max()，以下是函数声明：

```c++
int max(int num1, int num2);
```

在函数声明中，参数的名称并不重要，只有参数的类型是必需的，因此下面也是有效的声明：

```c++
int max(int, int);
```

当您在一个源文件中定义函数且在另一个文件中调用函数时，函数声明是必需的。在这种情况下，您应该在调用函数的文件顶部声明函数。

## 调用函数

创建 C++ 函数时，会定义函数做什么，然后通过调用函数来完成已定义的任务。

当程序调用函数时，程序控制权会转移给被调用的函数。被调用的函数执行已定义的任务，当函数的返回语句被执行时，或到达函数的结束括号时，会把程序控制权交还给主程序。

调用函数时，传递所需参数，如果函数返回一个值，则可以存储返回值。例如：

```c++
#include <iostream>
using namespace std;
 
// 函数声明
int max(int num1, int num2);
 
int main ()
{
   // 局部变量声明
   int a = 100;
   int b = 200;
   int ret;
 
   // 调用函数来获取最大值
   ret = max(a, b);
 
   cout << "Max value is : " << ret << endl;
 
   return 0;
}
 
// 函数返回两个数中较大的那个数
int max(int num1, int num2) 
{
   // 局部变量声明
   int result;
 
   if (num1 > num2)
      result = num1;
   else
      result = num2;
 
   return result; 
}
```



# 局部对象

## 自动对象

默认情况下，局部变量的生命期局限于所在函数的每次执行期间。只有当定 
义它的函数被调用时才存在的对象称为自动对象。自动对象在每次调用函数时创 
建和撤销。

局部变量所对应的自动对象在函数控制经过变量定义语句时创建。如果在定 
义时提供了初始化式，那么每次创建对象时，对象都会被赋予指定的初值。对于 
未初始化的内置类型局部变量，其初值不确定。当函数调用结束时，自动对象就 
会撤销。

形参也是自动对象。形参所占用的存储空间在调用函数时创建，而在函数结 
束时撤销。

自动对象，包括形参，都在定义它们的块语句结束时撤销。形参在函数块中 
定义，因此当函数的执行结束时撤销。当函数结束时，会释放它的局部存储空间。 
在函数结束后，自动对象和形参的值都不能再访问了。

## 静态局部对象

一个变量如果位于函数的作用域内，但生命期跨越了这个函数的多次调用，
这种变量往往很有用。则应该将这样的对象定义为 **static(静态的)**。

static 局部对象确保不迟于在程序执行流程第一次经过该对象的定义语句 
时进行初始化。这种对象一旦被创建，在程序结束前都不会撤销。当定义静态局 
部对象的函数结束时，静态局部对象不会撤销。在该函数被多次调用的过程中， 
静态局部对象会持续存在并保持它的值。考虑下面的小例子，这个函数计算了自 
己被调用的次数:

```c++
size_t count_calls()
     {
          static size_t ctr = 0; // value will persist across calls
          return ++ctr;
     }
int main() 
    {
         for (size_t i = 0; i != 10; ++i)
             cout << count_calls() << endl;
        return 0; 
    }

```

这个程序会依次输出 1 到 10(包含 10)的整数。

在第一次调用函数 `count_calls` 之前，`ctr` 就已创建并赋予初值 `0`。每次 
函数调用都使加 `1`，并且返回其当前值。在执行函数 `count_calls` 时，变量 `ctr` 
就已经存在并且保留上次调用该函数时的值。因此，第二次调用时，ctr 
的值为 `1`，第三次为 `2`，依此类推。

# 函数参数

## 形式参数

如果函数要使用参数，则必须声明接受参数值的变量。这些变量称为函数的**形式参数**。

形式参数就像函数内的其他局部变量，在进入函数时被创建，退出函数时被销毁。

当调用函数时，有三种向函数传递参数的方式：

| 调用类型                                                     | 描述                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [传值调用](https://www.runoob.com/cplusplus/cpp-function-call-by-value.html) | 该方法把参数的实际值赋值给函数的形式参数。在这种情况下，修改函数内的形式参数对实际参数没有影响。 |
| [指针调用](https://www.runoob.com/cplusplus/cpp-function-call-by-pointer.html) | 该方法把参数的地址赋值给形式参数。在函数内，该地址用于访问调用中要用到的实际参数。这意味着，修改形式参数会影响实际参数。 |
| [引用调用](https://www.runoob.com/cplusplus/cpp-function-call-by-reference.html) | 该方法把参数的引用赋值给形式参数。在函数内，该引用用于访问调用中要用到的实际参数。这意味着，修改形式参数会影响实际参数。 |

默认情况下，C++ 使用**传值调用**来传递参数。一般来说，这意味着函数内的代码不能改变用于调用函数的参数。之前提到的实例，调用 max() 函数时，使用了相同的方法。

## 参数的默认值

当您定义一个函数，您可以为参数列表中后边的每一个参数指定默认值。当调用函数时，如果实际参数的值留空，则使用这个默认值。

这是通过在函数定义中使用赋值运算符来为参数赋值的。调用函数时，如果未传递参数的值，则会使用默认值，如果指定了值，则会忽略默认值，使用传递的值。请看下面的实例：

```c++
#include <iostream>
using namespace std;
 
int sum(int a, int b=20)
{
  int result;
 
  result = a + b;
  
  return (result);
}
 
int main ()
{
   // 局部变量声明
   int a = 100;
   int b = 200;
   int result;
 
   // 调用函数来添加值
   result = sum(a, b);
   cout << "Total value is :" << result << endl;
 
   // 再次调用函数
   result = sum(a);
   cout << "Total value is :" << result << endl;
 
   return 0;
}
```

当上面的代码被编译和执行时，它会产生下列结果：

```
Total value is :300
Total value is :120
```

# Lambda 函数

C++11 提供了对匿名函数的支持,称为 Lambda 函数(也叫 Lambda 表达式)。

Lambda 表达式把函数看作对象。Lambda 表达式可以像对象一样使用，比如可以将它们赋给变量和作为参数传递，还可以像函数一样对其求值。

Lambda 表达式本质上与函数声明非常类似。Lambda 表达式具体形式如下:

```
[capture](parameters)->return-type{body}
```

例如：

```
[](int x, int y){ return x < y ; }
```

如果没有返回值可以表示为：

```
[capture](parameters){body}
```

例如：

```
[]{ ++global_x; } 
```

在一个更为复杂的例子中，返回类型可以被明确的指定如下：

```
[](int x, int y) -> int { int z = x + y; return z + x; }
```

本例中，一个临时的参数 z 被创建用来存储中间结果。如同一般的函数，z 的值不会保留到下一次该不具名函数再次被调用时。

如果 lambda 函数没有传回值（例如 void），其返回类型可被完全忽略。

在Lambda表达式内可以访问当前作用域的变量，这是Lambda表达式的闭包（Closure）行为。 与JavaScript闭包不同，C++变量传递有传值和传引用的区别。可以通过前面的[]来指定：

```
[]      // 沒有定义任何变量。使用未定义变量会引发错误。
[x, &y] // x以传值方式传入（默认），y以引用方式传入。
[&]     // 任何被使用到的外部变量都隐式地以引用方式加以引用。
[=]     // 任何被使用到的外部变量都隐式地以传值方式加以引用。
[&, x]  // x显式地以传值方式加以引用。其余变量以引用方式加以引用。
[=, &z] // z显式地以引用方式加以引用。其余变量以传值方式加以引用。
```

另外有一点需要注意。对于[=]或[&]的形式，lambda 表达式可以直接使用 this 指针。但是，对于[]的形式，如果要使用 this 指针，必须显式传入：

```
[this]() { this->someFunc(); }();
```





{%note info 重要概念%}
函数名：记住命名规则，包含数字、字符串和`_`,不可以以数字开头
形参：为函数提供了已命名的局部存储空间。
函数体:是一个作用域.
返回值：返回类型可以是内置类型(如 int 或者 double)、类类型或复合类型(如 int& 或 string*)，还可以是 void 类型，表示该函数不返回任何值。

局部变量: 在函数体内定义的变量只在该函数中才可以访问。
形参和实参：

* 形参：为函数提供了已命名的局部存储空间。（定义时使用）
* 实参：则是一个表达式。它可以是变量或字面值常量，甚至是包含一个或几个操作符的表达式。（调用时使用）
{% endnote %}

#  函数术语

| 术语 | 解释 |
| ---- | ---- |
|ambiguous call(有二义性的调用)| 一种编译错误，当调用重载函数，找不到唯一的最佳匹配时产生。|
|arguments(实参)|调用函数时提供的值。这些值用于初始化相应的形参，其方式类似于初始 化同类型变量的方法。|
|automatic objects(自动对象)|局部于函数的对象。自动对象会在每一次函数调用时重新创建和初始化， 并在定义它的函数块结束时撤销。一旦函数执行完毕，这些对象就不再存 在了。|
|best match(最佳匹配) |在重载函数集合里找到的与给定调用的实参达到最佳匹配的唯一函数。|
|call operator(调用操作符)|使函数执行的操作符。该操作符是一对圆括号，并且有两个操作数:被调 用函数的名字，以及由逗号分隔的(也可能为空)形参表。|
|candidate functions(候选函数)|在解析函数调用时考虑的函数集合。候选函数包括了所有在该调用发生的 作用域中声明的、具有该调用所使用的名字的函数。|
|const member function(常量成员函数)|类的成员函数，并可以由该类类型的常量对象调用。常量成员函数不能修 改所操纵的对象的数据成员。|
|constructor(构造函数)|与所属类同名的类成员函数。构造函数说明如何初始化本类的对象。构造 函数没有返回类型，而且可以重载。|
|constructor initializer list(构造函数初始化列表)|在构造函数中用于为数据成员指定初值的表。初始化列表出现在构造函数 的定义中，位于构造函数体与形参表之间。该表由冒号和冒号后面的一组 用逗号分隔的成员名组成，每一个成员名后面跟着用圆括号括起来的该成 员的初值。|
|default constructor(默认构造函数)|在没有显式提供初始化式时调用的构造函数。如果类中没有定义任何构造 函数，编译器会自动为这个类合成默认构造函数。|
|function(函数)| 可调用的计算单元。|
|function body(函数体) |定义函数动作的语句块。|
|function matching(函数匹配)|确定重载函数调用的编译器过程。调用时使用的实参将与每个重载函数的 形参表作比较。|
|function prototype(函数原型)|函数声明的同义词。包括了函数的名字、返回类型和形参类型。调用函数 时，必须在调用点之前声明函数原型。|
|inline function(内联函数)|如果可能的话，将在调用点展开的函数。内联函数直接以函数代码替代了 函数调用点展开的函数。内联函数直接以函数代码替代了函数调用语句， 从而避免了一般函数调用的开销。|
|local static objects(局部静态对象)|在函数第一次调用前就已经创建和初始化的局部对象，其值在函数的调用之间保持有效。|
|local variables(局部变量) |在函数内定义的变量，仅能在函数体内访问。|
|object lifetime(对象生命期)|每个对象皆有与之关联的生命期。在块中定义的对象从定义时开始存在， 直到它的定义所在的语句块结束为止。静态局部对象和函数外定义的全局 变量则在程序开始执行时创建，当 main 函数结束时撤销。动态创建的对 象由 new 表达式创建，从此开始存在，直到由相应的 delete 表达式释 放所占据的内存空间为止。|
|overload resolution(重载确定) |函数匹配的同义词。|
|overloaded function(重载函数)|和至少一个其他函数同名的函数。重载函数必须在形参的个数或类型上有 所不同。|
|parameters(形参) |函数的局部变量，其初值由函数调用提供。|
|recursive function(递归函数)| 直接或间接调用自己的函数。|
|return type(返回类型) |函数返回值的类型。|
|synthesized default constructor(合成默认构造函数)|如果类没有定义任何构造函数，则编译器会为这个类创建(合成)一个默 认构造函数。该函数以默认的方式初始化类中的所有数据成员。|
|temporary object(临时对象)|在求解表达式的过程中由编译器自动创建的没有名字的对象。“临时对 象”这个术语通常简称为“临时”。临时对象一直存在直到最大表达式结束为止，最大表达式指的是包含创建该临时对象的表达式的最大范围内的 表达式。|
|this pointer(this 指针)|成员函数的隐式形参。this 指针指向调用该函数的对象，是指向类类型 的指针。在 const 成员函数中，该指针也指向 const 对象。|
|viable functions(可行函数)|重载函数中可与指定的函数调用匹配的子集。可行函数的形参个数必须与 该函数调用的实参个数相同，而且每个实参类型都可潜在地转换为相应形参的类型。|





# 课堂实践

## 实践1

要求：解释形参、局部变量和静态局部变量的差别。并给出一个有效使用了这三种变量的程序例子。

## 实践2