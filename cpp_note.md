# C++笔记

## 基本语法

### sizeof是函数吗

在 C++ 中，sizeof 不是函数，而是一个运算符（Operator）。它用于计算对象或类型的大小（以字节为单位）。sizeof 运算符在编译时求值。

使用 sizeof 运算符时，有两种不同的语法：

1. sizeof(expression)
这将返回 expression 所占内存的大小。expression 可以是任何变量、数据类型、表达式或对象。

2. sizeof(type)
这将返回 type 的大小，即数据类型本身所占的内存大小。type 可以是任何数据类型，如 int、double、char、自定义的结构体等。

下面是一些使用 sizeof 运算符的例子

```cpp
#include <iostream>

int main() {
    int num = 42;
    double pi = 3.14159;
    char ch = 'A';

    std::cout << "Size of int: " << sizeof(int) << " bytes" << std::endl;
    std::cout << "Size of double: " << sizeof(double) << " bytes" << std::endl;
    std::cout << "Size of char: " << sizeof(char) << " bytes" << std::endl;

    std::cout << "Size of num: " << sizeof(num) << " bytes" << std::endl;
    std::cout << "Size of pi: " << sizeof(pi) << " bytes" << std::endl;
    std::cout << "Size of ch: " << sizeof(ch) << " bytes" << std::endl;

    return 0;
}
```

请注意，sizeof 的结果是在编译时确定的，并且可能因编译器和目标平台的不同而有所变化。

### const和constexpr

#### const与constexpr的区别

被const修饰的变量带有两种属性：只读和常量。而被constexpr修饰的变量只有常量这一属性。

#### 指针常量 vs. 常量指针

##### 指针常量

英文是pointer to constant,即指向常量的指针，它表示该指针指向的数据是个常量，不能通过该指针修改。但指针本身是可以修改指向其他常量的。使用const关键字在指针符号'*'前声明指针常量，例如：

```cpp
const int* ptr; // ptr 是指向整数常量的指针
// 或者
int const* ptr; // 同样是指向整数常量的指针
```

##### 常量指针

英文是constant pointer，它表示指针本身就是一个常量，一旦被初始化就不允许再指向其它对象。但可以通过指针修改它所指向的对象。需要注意的是，在类中声明常量指针时，一定要在构造函数初始化列表完成对常量指针的初始化。
  
使用 const 关键字在指针前声明常量指针，例如：

```cpp
int num = 42;
int* const ptr = &num; // ptr 是一个指向整数的常量指针
```

#### const成员函数

- 被const修饰的成员函数不允许修改其成员变量。
- 常量对象和非常量对象均可调用。
- 如果函数签名一样，只不过一个为const，一个为非const，那么会根据对象是否为const调用对应的函数。
- 常量对象可以调用非const成员函数，但如果修改数据成员，则会发生编译器错误。

#### constexpr函数

被constexpr修饰的函数将在编译器进行求值，从而提高程序的性能。以下是一个具体示例

```cpp
constexpr int square(int x) {
    return x * x;
}

constexpr int result = square(5); // 在编译时计算 square(5)，结果为 25
```

##### 要点

- constexpr 函数必须满足一定的条件，即函数的输入参数和返回值类型都必须是可计算的常量表达式。在 C++11 中，constexpr 函数通常被限制为只有一个 return 语句。

- 在 C++14 中，constexpr 函数的限制有所放宽，允许函数内部包含多个语句和控制流程（如循环和条件语句），只要在编译时可以确定结果。

```cpp
#include <iostream>

constexpr int factorial(int n) {
    return (n <= 0) ? 1 : n * factorial(n - 1);
}

int main() {
    constexpr int num = 5;
    constexpr int result = factorial(num); // result将在编译器已知
    std::cout << "Factorial of " << num << " is " << result << std::endl;
    return 0;
}
```

##### 优点

- 使用 constexpr 可以帮助编译器进行优化，将常量表达式的计算提前到编译期间，而不需要在运行时计算，从而提高程序的性能。

- constexpr 还能够让程序员明确表达意图，将一些值标记为常量，并帮助编译器进行更严格的类型检查。

#### 编译器优化：常量折叠和常量传播

- 常量折叠：编译器在编译期间可以对常量表达式进行计算，并将其替换为计算后的常量值。

```cpp
const int x = 5;
int result = x * 2; // x * 2在编译阶段计算其值
```

- 常量传播：编译器可以将常量的值传播到使用常量的地方，以减少对常量对象的读取次数。这可以减少内存访问和运行时计算的开销，提高程序的执行效率。如果没有通过引用指向常量，编译器可以不为常量分配内存。

```cpp
constexpr int x = 5;
int result = x + 10; // x被替换为5
```

### inline vs. define

### multable

- 被multable修饰的变量可以摆脱const成员函数的限制，对其进行修改。
- multable关键字只能修饰非静态成员变量，不能修饰函数、静态成员变量等。

### volatile

### enum与enum class

- enum中的成员是全局可见的，存在污染外部作用域的问题。

- enum class中的成员作用域局限于类内，而且可以定义enum class的类型，比如int，short等。

## 面向对象编程

### 面向对象的三大特性

#### 封装

#### 继承

#### 多态

### 函数重载

### 运算符重载

讲述哪些运算符可以重载，哪些不可以重载
哪些运算符只能是成员函数或非成员函数
匹配规则：通用函数匹配规则。但是有一点，在表达式中，无法根据调用形式判断是使用成员函数还是非成员函数。因为它们均在候选集中。

## 现代C++

### 智能指针

#### unique_ptr

#### shared_ptr

#### weak_ptr

## STL container

这里主要介绍STL容器的基本原理、使用技巧以及注意事项。

### vector

#### 为什么不建议使用`vector<bool>`

#### reserve和resize

- reserve改变的是vector的capabilty，而非size
- resize既改变vector的capability，也改变size

#### 迭代器失效

##### 原因

在插入元素时，vector可能会发生内存的重新分配，原内存中的元素会赋值或移动到新内存。这将使原来的迭代器失效，比如尾迭代器。

##### 解决方案

重新获取失效的迭代器，尤其是尾迭代器。

### list

#### list.remove和alogrithm.remove

之所以提这两个函数呢，是因为list的remove接口特别sb。它的含义和algorithm中remove函数的含义有所不同：

- list.remove它的作用是删除值为val的元素，它会将节点从list中移除，改变list的size。
- algorithm.remove的作用和list.remove相同，但是它不会将节点从list中移除，不会改变list的size。

### deque

### map vs. set

之所以放在一起，是因为其底层数据结构是一样的。

#### 为什么只需重载小于运算符就可以实现自定义类作为key

因为a == b可以通过(a < b) && !(b < a)来实现。这样一来可以减少对自定义类的要求。

### unordered_map vs. unordered_set

#### 自定义类作为key

- 提供重载相等运算符
- 提供hash函数

## 对象模型

## 模板
