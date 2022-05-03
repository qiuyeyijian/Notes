# 类

## 基础知识

对象的复制，就是定义一个新对象时，用另外一个老对象里面的内容初始化。

默认参都必须出现在非默认参数的右侧，一旦开始为某个参数指定默认值，则它右侧的所有参数都必须指定默认值。



### 隐式转换和explicit

```cpp
class Time {
 private:
  int a;

 public:
  // 加上explicit 表示不能利用构造函数进行隐式转换
  explicit Time(int x);
};
// 类内声明，类外定义（实现）
Time::Time(int x) {
    a = x;
}

int main(int argc, char const *argv[]) {
  /* 以下两条语句去掉构造函数前面的explicit 就正确了 */
  //   Time t = {1};      错误，进行了隐式类型转换
  //    Time t = 1;         错误，进行了隐式类型转换

  return 0;
}
```



### 构造函数初始化列表

初始化列表在函数体执行之前就执行，提倡优先使用初始化列表，而不是使用传统的构造函数体内赋值。因为这样可以减少不必要的函数调用、重载等操作，效率更高。

```cpp
class Time {
 private:
  int ta, tb;

 public:
  Time() : ta(0), tb(0){}
  Time(int x, int y);
};

// 这种写法只能用于构造函数, 且初始化顺序和成员变量定义顺序相同
Time::Time(int x, int y) : ta(x), tb(y) {
    //...
}
```



## inline、const、mutable、this与static

### 成员函数inline

一般推荐是类内声明，类外实现。但是如果在类内实现成员函数，则该成员函数会被作为inline内联函数处理，所以在类内实现的函数要尽可能简单。

### 成员函数末尾的const

如果告诉系统，该成员函数不会修改对象里面的任何成员变量，也就是不会修改类对象的任何状态，则在成员函数的声明和实现末尾都要加const。

```cpp
class Time {
 private:
  int ta, tb;

 public:
  Time(int x, int y);
  void print() const;
};

// 构造函数初始化列表
Time::Time(int x, int y) : ta(x), tb(y) {}
// const成员函数
void Time::print() const { printf("ta:%d, tb:%d", ta, tb); }

int main(int argc, char const *argv[]) {
  const Time t1(1, 2);  // const 对象只能调用const成员函数
  Time t2(1, 2);        // 非const对象只要权限允许，都可以调用
  return 0;
}
```

**普通函数（非成员函数）末尾不能加const，编译无法通过。**因为const在函数末尾的意思是“成员函数不会修改该对象里面任何成员变量的值”，普通函数没有对象的概念。



### mutable

const成员函数有其存在的价值，比如可以被const对象调用。如果设计一个能被const对象调用的const成员函数，同时又希望能修改成员变量的值，则需要用mutable修饰成员变量。

```cpp
mutable int tc;

void modify() const {
    tc++;
}
```



### 返回自身对象的引用 this

this用在成员函数中是一个隐藏起来的函数参数，表示的是指向本对象的指针，*this表示该对象。

this本身是一个指针常量，总是指向这个对象本身。

this指针只能在普通成员函数中使用，全局函数、静态函数等都不能使用。



利用this可以实现链式调用

```cpp
// 添加成员函数声明
Time& add(int z);

// 类外实现该成员函数
Time& Time::add(int z) {
  ta += z;
  tb += z;

  return *this;
}

// 测试
Time t(1, 1);
// 首先给成员变量加10，然后打印
t.add(10).print();
```



## 重载运算符、拷贝赋值运算符与析构函数

### 重载运算符

重载运算符本质上是函数，名字是`operator+运算符`，例如`operator==`。

```cpp
bool operator==(Time& t);

// 重载 == 运算符
bool Time::operator ==(Time& t) {
   // ...
}

// 重载赋值运算符
Time& Time::operator=(const Time& t) {
    //...
    return *this;
}
```



### 子类、调用顺序、访问等级与函数遮蔽

```cpp
class Human {
 private:
  int age;
  string name;

 public:
  Human();
  Human(int x);
};

class Man : public Human {
 private:
 public:
  Man();
};
```

**先执行父类的构造函数，再执行子类的构造函数。**

对于父类来讲，尤其是父类的成员函数：

* private：不想让外面访问
* protect：想让自己的子类访问
* public：公开访问

在C++继承中，子类会遮蔽父类中的同名函数。要想调用父类函数就需要使用双冒号指明作用域了。



## 父类指针、虚函数、多态性与析构函数

当创建一个子类的时候，系统先调用父类的构造函数，再调用子类的构造函数。当销毁该对象时，先调用子类的析构函数，在调用父类的析构函数。

用派生类对象为一个基类对象初始化或者赋值时，只有该派生类对象的基类部分会被复制或者赋值，派生类部分将被忽略掉。

### 父类指针与子类指针

父类指针可以指向一个子类对象，反之不行。

```cpp
Human* h = new Man();
```



### 虚函数

用父类的指针调用一个虚成员函数时，执行的是动态绑定。所谓动态，表示的就是在程序运行的时候（运行到调用eat函数这行代码时）才能知道调用了哪个子类的eat函数（虚成员函数）。

由于是在程序运行的时候才知道调用了哪个虚函数，所以虚函数必须写他的定义部分。

```cpp
class Human {
 private:
  int age;
  string name;

 public:
  virtual void eat();
  //   virtual void eat() final;      // 如果后面加final，则子类不能再覆盖此函数
};

class Male : public Human {
 private:
 public:
  // 会覆盖父类中的同名同参虚函数
  virtual void eat() override;
};

class Female : public Human {
 private:
 public:
  // 会覆盖父类中的同名同参虚函数
  virtual void eat() override;
};

void Human::eat() { printf("Human\n"); }
void Male::eat() { printf("Male\n"); }
void Female::eat() { printf("Female\n"); }

int main(int argc, char const* argv[]) {
  Human* human_ptr = new Human;
  human_ptr->eat(); /* Human */
  delete human_ptr;

  human_ptr = new Male;
  human_ptr->eat(); /* Male */

  human_ptr->Human::eat(); /* Human */
  delete human_ptr;

  return 0;
}
```



### 多态性

多态性知识针对虚函数说的。多态性体现在：

（1）子类重新定义（覆盖/重写）父类的成员函数，同时父类和子类中又把这个成员函数声明为了virtual虚函数。

（2）根据具体执行到的代码行，才能找到动态绑定到父类指针的对象，这个对象可以是父类对象，也可以是子类对象。之后，系统内部实际上要查类的“虚函数表”，根据虚函数表找到虚函数的入口地址。



### 纯虚函数与抽象类

纯虚函数是在父类中声明的虚函数：

```cpp
virtual void eat() = 0;			// 这是一个纯虚函数
```

这种带有纯虚函数的类就叫做抽象类。抽象类不能生成对象，主要目的是统一管理子类（或者说建立一些供子类参照的标准或规范）

抽象类主要用来做父类，定义一些公共接口。**子类中必须要实现父类（抽象类）中定义的纯虚函数。**抽象类中的函数不写函数实现，推迟到子类中去写。

多态的实现是：父类指针指向子类对象。如果没有父类，也就不存在多态。纯虚函数也是虚函数，因此也是支持多态的。



### 父类的析构函数一般写成虚函数

**唯有这样，当delete一个指向子类对象的父类指针是，才能保证系统能够依次调用子类的析构函数和父类的析构函数，从而保证对象内存被正确地释放。**

```cpp
vitrual ~Human();
```

用父类指针new一个子类对象，在delete的时候系统不会调用子类的析构函数，这就说明内存没有回收干净，存在内存泄露。

只有虚函数才能够做到用父类指针调用子类的虚函数。也正是由于这种特性，所以只要把析构函数声明为虚函数，系统内部就能够正确处理调用关系。

父类中析构函数的虚属性也会被继承给子类，C++中为了获得运行时的多态行为，所调用的成员函数必须是virtual的。

> 1. 如果一个类想要做父类，务必给这个类写虚析构函数，否则会造成内存泄露问题。
>
> 2. 虚函数会增加内存和执行效率上的开销，类里面定义虚函数，**编译器就会给这个类增加虚函数表**，则个表里面存放的是虚函数地址等信息。虚函数表里有很多表项，每一项都是一个指针，每个指针指向这个类里的各个虚函数的入口地址。



### RTTI、dynamic_cast、typeid、type_info与虚函数表

### RTTI是什么

RTTI（Run Time Type Identification），运行时类型识别。也就是通过运行时类型识别，程序能够使用父类（基类）的指针或引用来检查这些指针或引用所指的对象的实际子（派生）类型。

RTTI可以看做系统提供出来的一种功能或者说是一种能力。这种功能或者能力通过两个运算符dynamic_cast 和 typeid 来实现，**而且父类中必须要有虚函数。**

### dynamic_cast

`dynamic_cast`：能将父类的指针或者引用安全地转换为子类的指针或者引用。

```cpp
Human* human_ptr = new Human;
Male* male = dynamic_cast<Male*>(human_ptr);
```

### typeid

`typeid`：返回指针或者引用所指对象的实际类型。通过这个运算符可以获取对象的类型信息。这个运算符会返回一个常量对象的引用，这个常量对象的类型一般是标准库类型 type_info，其实type_info就是一个类。



### type_info

type_info有个成员函数`name()`，用于获取类型名字信息。重载了两个运算符：`==`和`!=`用来比较类型是否相同。



































































































































































