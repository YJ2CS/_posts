---
title: 第七章 类 （Class）
siteurl: ch07-第七章-类-class
author: YJ2CS
avatar: '/custom/avatar.webp'
authorLink: YJ2CS.github.io
authorAbout: 愿青年摆脱了冷气，只是向前走！
authorDesc: 愿青年摆脱了冷气，只是向前走！
comments: true
categories:
  - learn_cpp
tags:
  - 悦读
no-photos: https://random.52ecy.cn/randbg.php?size=1&rid-553e
date: 2020-11-10 22:16:00
---

# 第七章 类 （Class）

## 定义抽象数据类型

- **类背后的基本思想**：**数据抽象**（data abstraction）和**封装**（encapsulation）。
- 数据抽象是一种依赖于**接口**（interface）和**实现**（implementation）分离的编程技术。

### 类成员 （Member）

- 必须在类的内部声明，不能在其他地方增加成员。
- 成员可以是数据，函数，类型别名。

### 类的成员函数

- 成员函数的**声明**必须在类的内部。
- 成员函数的**定义**既可以在类的内部也可以在外部。
- 使用点运算符 `.` 调用成员函数。
- 必须对任何`const`或引用类型成员以及没有默认构造函数的类类型的任何成员使用初始化式。
- `ConstRef::ConstRef(int ii): i(ii), ci(i), ri(ii) { }`
- 默认实参： `Sales_item(const std::string &book): isbn(book), units_sold(0), revenue(0.0) { }`
- `*this`：
  - 每个成员函数都有一个额外的，隐含的形参`this`。
  - `this`总是指向当前对象，因此`this`是一个常量指针。
  - 形参表后面的`const`，改变了隐含的`this`形参的类型，如 `bool same_isbn(const Sales_item &rhs) const`，这种函数称为“常量成员函数”（`this`指向的当前对象是常量）。
  - `return *this;`可以让成员函数连续调用。
  - 普通的非`const`成员函数：`this`是指向类类型的`const`指针（可以改变`this`所指向的值，不能改变`this`保存的地址）。
  - `const`成员函数：`this`是指向const类类型的`const`指针（既不能改变`this`所指向的值，也不能改变`this`保存的地址）。

### 非成员函数

- 和类相关的非成员函数，定义和声明都应该在类的外部。

### 类的构造函数

- 类通过一个或者几个特殊的成员函数来控制其对象的初始化过程，这些函数叫做**构造函数**。
- 构造函数是特殊的成员函数。
- 构造函数放在类的`public`部分。
- 与类同名的成员函数。
- `Sales_item(): units_sold(0), revenue(0.0) { }`
- `=default`要求编译器合成默认的构造函数。(`C++11`)
- 初始化列表：冒号和花括号之间的代码： `Sales_item(): units_sold(0), revenue(0.0) { }`

## 访问控制与封装

- **访问说明符**（access specifiers）：
  - `public`：定义在 `public`后面的成员在整个程序内可以被访问； `public`成员定义类的接口。
  - `private`：定义在 `private`后面的成员可以被类的成员函数访问，但不能被使用该类的代码访问； `private`隐藏了类的实现细节。
- 使用 `class`或者 `struct`：都可以被用于定义一个类。唯一的却别在于访问权限。
  - 使用 `class`：在第一个访问说明符之前的成员是 `priavte`的。
  - 使用 `struct`：在第一个访问说明符之前的成员是 `public`的。

### 友元

- 允许特定的**非成员函数**访问一个类的**私有成员**.
- 友元的声明以关键字 `friend`开始。 `friend Sales_data add(const Sales_data&, const Sales_data&);`表示非成员函数`add`可以访问类的非公有成员。
- 通常将友元声明成组地放在**类定义的开始或者结尾**。
- 类之间的友元：
  - 如果一个类指定了友元类，则友元类的成员函数可以访问此类包括非公有成员在内的所有成员。

### 封装的益处

- 确保用户的代码不会无意间破坏封装对象的状态。
- 被封装的类的具体实现细节可以随时改变，而无需调整用户级别的代码。

## 类的其他特性

- 成员函数作为内联函数 `inline`：
  - 在类的内部，常有一些规模较小的函数适合于被声明成内联函数。
  - **定义**在类内部的函数是**自动内联**的。
  - 在类外部定义的成员函数，也可以在声明时显式地加上 `inline`。
- **可变数据成员** （mutable data member）：
  - `mutable size_t access_ctr;`
  - 永远不会是`const`，即使它是`const`对象的成员。
- **类类型**：
  - 每个类定义了唯一的类型。

## 类的作用域

- 每个类都会定义它自己的作用域。在类的作用域之外，普通的数据和函数成员只能由引用、对象、指针使用成员访问运算符来访问。
- 函数的**返回类型**通常在函数名前面，因此当成员函数定义在类的外部时，返回类型中使用的名字都位于类的作用域之外。
- 如果成员使用了外层作用域中的某个名字，而该名字代表一种**类型**，则类不能在之后重新定义该名字。
- 类中的**类型名定义**都要放在一开始。

## 构造函数再探

- 构造函数初始值列表：
  - 类似`python`使用赋值的方式有时候不行，比如`const`或者引用类型的数据，只能初始化，不能赋值。（注意初始化和赋值的区别）
  - 最好让构造函数初始值的顺序和成员声明的顺序保持一致。
  - 如果一个构造函数为所有参数都提供了默认参数，那么它实际上也定义了默认的构造函数。

### 委托构造函数 （delegating constructor, `C++11`）

- 委托构造函数将自己的职责委托给了其他构造函数。
- `Sale_data(): Sale_data("", 0, 0) {}`

### 隐式的类型转换

- 如果构造函数**只接受一个实参**，则它实际上定义了转换为此类类型的**隐式转换机制**。这种构造函数又叫**转换构造函数**（converting constructor）。
- 编译器只会自动地执行`仅一步`类型转换。
- 抑制构造函数定义的隐式转换：
  - 将构造函数声明为`explicit`加以阻止。
  - `explicit`构造函数只能用于直接初始化，不能用于拷贝形式的初始化。

### 聚合类 （aggregate class）

- 满足以下所有条件：
  - 所有成员都是`public`的。
  - 没有定义任何构造函数。
  - 没有类内初始值。
  - 没有基类，也没有`virtual`函数。
- 可以使用一个花括号括起来的成员初始值列表，初始值的顺序必须和声明的顺序一致。

### 字面值常量类

- `constexpr`函数的参数和返回值必须是字面值。
- **字面值类型**：除了算术类型、引用和指针外，某些类也是字面值类型。
- 数据成员都是字面值类型的聚合类是字面值常量类。
- 如果不是聚合类，则必须满足下面所有条件：
  - 数据成员都必须是字面值类型。
  - 类必须至少含有一个`constexpr`构造函数。
  - 如果一个数据成员含有类内部初始值，则内置类型成员的初始值必须是一条常量表达式；或者如果成员属于某种类类型，则初始值必须使用成员自己的`constexpr`构造函数。
  - 类必须使用析构函数的默认定义，该成员负责销毁类的对象。

## 类的静态成员

- 非`static`数据成员存在于类类型的每个对象中。
- `static`数据成员独立于该类的任意对象而存在。
- 每个`static`数据成员是与类关联的对象，并不与该类的对象相关联。
- 声明：
  - 声明之前加上关键词`static`。
- 使用：
  - 使用**作用域运算符**`::`直接访问静态成员:`r = Account::rate();`
  - 也可以使用对象访问：`r = ac.rate();`
- 定义：
  - 在类外部定义时不用加`static`。
- 初始化：
  - 通常不在类的内部初始化，而是在定义时进行初始化，如 `double Account::interestRate = initRate();`
  - 如果一定要在类内部定义，则要求必须是字面值常量类型的`constexpr`。
  

# C++ Primer Chapter7 类（1）
第七章 类
声明: 本文为《C++ Primer 中文版（第五版）》学习笔记。 原书更为详细，本文仅作学习交流使用。

P228-P273

类的基本思想是数据抽象和封装。

抽象是一种依赖于接口和实现分离的编程技术。

封装实现了类的接口和实现的分离。

7.1 定义抽象数据类型
（1）this

任何对类成员的直接访问都被看作this的隐式引用。

std::string isbn() const {return bookNo;}
等价于

std::string isbn() const {return this->bookNo;}
（2）在类的外部定义成员函数

类外部定义的成员的名字必须包含它所属的类名。

double Sales_data::avg_price() const {
    if (units_sol)
        return revenue/units_sols;
    else
        return 0;
}
（3）构造函数

定义：类通过一个或几个特殊的成员函数来控制其对象的初始化过程，这些函数叫做构造函数。
构造函数没有返回类型；
构造函数的名字和类名相同。
类通过一个特殊的构造函数来控制默认初始化过程，这个函数叫做默认构造函数。

编译器创建的构造函数被称为合成的默认构造函数。

::: tip

只有当类没有声明任何构造函数的时，编译器才会自动的生成默认构造函数。

一旦我们定义了一些其他的构造函数，除非我们再定义一个默认的构造函数，否则类将没有默认构造函数

:::

7.2 访问控制与封装
（1）访问控制

| 说明符 | 用途 |
| ------- | ------------------------------------------------------------ |
| public | 使用public定义的成员，在整个程序内可被访问，public成员定义类的接口。 |
| private | 使用private定义的成员可以被类的成员函数访问，但是不能被使用该类的代码访问，private部分封装了类的实现细节。 |

（2）友元

类可以允许其他类或者函数访问它的非公有成员，方法是令其他类或者函数成为它的友元。

以friend关键字标识。

友元不是类的成员，不受访问控制级别的约束。

::: tip

友元的声明仅仅制定了访问的权限，而非通常意义的函数声明。必须在友元之外再专门对函数进行一次声明。

:::

// Sales_data.h

class Sales_data {
friend Sales_data add(const Sales_data&, const Sales_data&);
friend std::ostream &print(std::ostream&, const Sales_data&);
friend std::istream &read(std::istream&, Sales_data&);
}

// nonmember Sales_data interface functions
Sales_data add(const Sales_data&, const Sales_data&);
std::ostream &print(std::ostream&, const Sales_data&);
std::istream &read(std::istream&, Sales_data&);

//Sales_data.cpp

Sales_data 
add(const Sales_data &lhs, const Sales_data &rhs)
{
    Sales_data sum = lhs;  // copy data members from lhs into sum
    sum.combine(rhs);      // add data members from rhs into sum
    return sum;
}

// transactions contain ISBN, number of copies sold, and sales price
istream&
read(istream &is, Sales_data &item)
{
    double price = 0;
    is >> item.bookNo >> item.units_sold >> price;
    item.revenue = price * item.units_sold;
    return is;
}

ostream&
print(ostream &os, const Sales_data &item)
{
    os << item.isbn() << " " << item.units_sold << " " 
       << item.revenue << " " << item.avg_price();
    return os;
}
7.3 类的其他特性
（1）重载成员变量

Screen myScrren;
char ch = myScreen.get();
ch = myScreen.get(0,0);
（2）类数据成员的初始化

类内初始值必须使用=或者{}的初始化形式。

class Window_mgr{
private:
    std::vector<Screen> screens{Screen(24, 80, ' ')};
}
（3）基于const的重载

class Screen {
public:
    // display overloaded on whether the object is const or not
    Screen &display(std::ostream &os) 
                  { do_display(os); return *this; }
    const Screen &display(std::ostream &os) const
                  { do_display(os); return *this; }
}
当某个对象调用display的时候，该对象是否是const决定了应该调用display的哪个版本。

（3）类类型

对于一个类来说，在我们创建他的对象之前该类必须被定义过，而不能仅被声明。

（4）友元

友元类

如果一个类指定了友元类，则友元类的成员函数可以访问此类包括非公有成员在内的所有成员。

class Screen {
    // Window_mgr的成员可以访问Screen类的私有部分
    friend class Window_mgr;
}
令成员函数作为友元

class Screen {
    // Window_mgr::clear必须在Screen类之前被声明
    friend void Window_mgr::clear(ScreenIndex);
}
