# C++11

## 原始字面量

在 C++11 中添加了定义原始字符串的字面量，定义方式为：`R“xxx(原始字符串)xxx”` 其中（）两边的字符串可以省略。原始字面量 R 可以直接表示字符串的实际含义，而不需要额外对字符串做转义或连接等操作。

```cpp
string str1 = "D:\\hello\\world\\test.text";
cout << str1 << endl;

string str2 = R"(D:\hello\world\test.text)";
cout << str2 << endl;
```

在`R“xxx(raw string)xxx”` 中，原始字符串必须用括号（）括起来，括号的前后可以加其他字符串，所加的字符串会被忽略，因此一般不指定，并且加的字符串必须在括号两边同时出现。

```cpp
string str2 = R"test(D:\hello\world\test.text)test";		// test会被忽略
cout << str2 << endl;
```

如果原始字符串中有包含`"( 或者 )"`，则括号前后需要加一些描述语句，否则造成错误。

```cpp
string str3 = R"test(D:\hello\"()"world\test.text)test";  // 随便加上一些唯一的描述字符串比如 test等
cout << str3 << endl;
```



## long long 类型

相比于 C++98 标准，C++11 整型的最大改变就是多了 long long。但事实上，long long 整型本来就离 C++ 标准很近，早在 1995 年，long long 就被提议写入 C++98 标准，却被 C++ 标准委员会拒绝了。而后来，long long 类型却进入了 C99 标准，而且也事实上也被很多编译器支持。于是辗转地，C++ 标准委员会又掉头决定将 long long 纳入 C++11 标准。

 C++11 标准规定该类型至少占 8 字节，它占的字节数越多，对应能够存储的数值也就越大。

```cpp
// 数字后面可以加 ll后缀，大小写或者混着写均可
long long n1 = 123ll;
unsigned long long n3 = 123ull;

cout << "long long 最大值：" << LLONG_MAX << endl;
cout << "long long 最小值：" << LLONG_MIN << endl;
cout << "unsigned long long 最大值：" << ULLONG_MAX << endl;
```

> * ` long long `、`long long int`、`signed long long`、`signed long long int`是等价的。
>
> * ` unsigned long long `和` unsigned long long int` 是等价的。

在 C++11 中一共只定义了以下 5 种标准的有符号整型：

 `signed char` 、`short int`、`int`、`long int`、`long long int`

标准同时规定，每一种有符号整型都有一种对应的无符号整数版本，且有符号整型与其对应的无符号整型具有相同的存储空间大小。比如与 signed int 对应的无符号版本的整型是 unsigned int。

当我们在 C++ 中处理数据的时候，如果参与运算的数据或者传递的参数类型不匹配，整型间会发生隐式的转换，这种过程通常被称为整型的提升。

> 关于这种整形提升的隐式转换遵循如下原则:
>
> * 长度越大的整型等级越高，比如 long long int 的等级会高于 int。
>
> * 长度相同的情况下，标准整型的等级高于扩展类型，比如 long long int 和 int64 如果 都是 64 位长度，则 long long int 类型的等级更高。
>
> * 相同大小的有符号类型和无符号类型的等级相同，long long int 和 unsigned long long int的等级就相同。
>
> * 转换过程中，低等级整型需要转换为高等级整型，有符号的需要转换为无符号整形。





## final和override

### final

C++ 中增加了 final 关键字来限制某个类不能被继承，或者某个虚函数不能被重写，和 Jave 的 final 关键字的功能是类似的。如果使用 final 修饰函数，只能修饰虚函数，并且要把final关键字放到类或者函数的后面。

```cpp
class Base {
 private:
 public:
  // 基类当中定义一个纯虚函数
  virtual void print() = 0;
};

// 使用final修饰类
class Child final : public Base {
 public:
    // 使用final修饰函数
  void print() final { cout << "hello" << endl; }
};

class GrandChild : public Child {	// 错误，不能继承被final修饰的类
 public:
  void print() {}		//错误，不能重写被final修饰过的虚函数
};
```



### override

override 关键字确保在派生类中声明的重写函数与基类的虚函数有相同的签名，同时也明确表明将会重写基类的虚函数，这样就可以保证重写的虚函数的正确性，也提高了代码的可读性，和 final 一样这个关键字要写到方法的后面。

```cpp
class Base {
 private:
 public:
  // 基类当中定义一个纯虚函数
  virtual void print() = 0;
};

class Child : public Base {
 public:
  void print() override { cout << "hello" << endl; }
};
```

使用了 override 关键字之后，假设在重写过程中因为误操作，写错了函数名或者函数参数或者返回值编译器都会提示语法错误，提高了程序的正确性，降低了出错的概率。







## 模板

C++模板分为类模板和函数模板。

### 默认模板参数

C++111添加了对函数模板默认参数的支持。也就是或类模板和函数模板都可以有默认模板参数。

当所有模板参数都有默认参数时，**函数模板的调用如同一个普通函数**。**但对于类模板而言，哪怕所有参数都有默认参数，在使用时也必须在模板名后跟随 <> 来实例化。**

```cpp
template <typename T = int, T t = 520>
class Test {
 public:
  void print() { cout << "current value: " << t << endl; }
};

int main() {
    // 类模板 <>不能省略
  Test<> t;
  t.print();
  return 0;
}
```



**函数模板的默认模板参数在使用规则上和其他的默认参数也有一些不同，它没有必须写在参数表最后的限制。**这样当默认模板参数和模板参数自动推导结合起来时，书写就显得非常灵活了。我们可以指定函数模板中的一部分模板参数使用默认参数，另一部分使用自动推导

```cpp
template <typename R = int, typename N>
R func(N arg) {
  return arg;
}

int main() {
  auto ret1 = func(520);
  cout << "return value-1: " << ret1 << endl;

  auto ret2 = func<double>(52.134);
  cout << "return value-2: " << ret2 << endl;

  auto ret3 = func<int>(52.134);
  cout << "return value-3: " << ret3 << endl;

  auto ret4 = func<char, int>(100);
  cout << "return value-4: " << ret4 << endl;

  return 0;
}

```

当默认模板参数和模板参数自动推导同时使用时（优先级从高到低）：

1. 如果可以推导出参数类型则使用推导出的类型

2. 如果函数模板无法推导出参数类型，那么编译器会使用默认模板参数

3. 如果无法推导出模板参数类型并且没有设置默认模板参数，编译器就会报错

```cpp
template <typename T, typename U = char>
void func(T arg1 = 100, U arg2 = 100) {
  cout << "arg1: " << arg1 << ", arg2: " << arg2 << endl;
}

int main() {
  // 模板函数调用
  func('a');
  func(97, 'a');
    
  // 虽然函数模板指定了默认参数，但是类型推导不会参考函数模板的默认参数
  // func();    //编译报错
  return 0;
}
```

* 模板参数类型的自动推导是根据模板函数调用时指定的实参进行推断的，没有实参则无法推导
* **模板参数类型的自动推导不会参考函数模板中指定的默认参数。**





## 数值和字符串转换

 使用 `to_string()` 方法可以非常方便地将各种数值类型转换为字符串类型。函数声明位于头文件`<string>`中。

```cpp
#include<string>
string str = to_string(1234);
```

### 字符串转为数值

由于 C++ 中的数值类型包括整形和浮点型，因此针对于不同的类型提供了不同的函数，通过调用这些函数可以将字符串类型转换为对应的数值类型。

```cpp
// 定义于头文件 <string>
int       stoi( const std::string& str, std::size_t* pos = 0, int base = 10 );
long      stol( const std::string& str, std::size_t* pos = 0, int base = 10 );
long long stoll( const std::string& str, std::size_t* pos = 0, int base = 10 );

unsigned long      stoul( const std::string& str, std::size_t* pos = 0, int base = 10 );
unsigned long long stoull( const std::string& str, std::size_t* pos = 0, int base = 10 );

float       stof( const std::string& str, std::size_t* pos = 0 );
double      stod( const std::string& str, std::size_t* pos = 0 );
long double stold( const std::string& str, std::size_t* pos = 0 );
```

> str：要转换的字符串
>
> pos：传出参数，记录从哪个字符开始无法继续进行解析，比如: 123abc, 传出的位置为 3
>
> base：若 base 为 0 ，则自动检测数值进制：若前缀为 0 ，则为八进制，若前缀为 0x 或 0X，则为十六进制，否则为十进制。

**这些函数虽然都有多个参数，但是除去第一个参数外其他都有默认值，一般情况下使用默认值就能满足需求。**

```cpp
int t1 = std::stoi("45");
float t2 = std::stof("3.1415");

int t3 = std::stoi("9821 test");
// int t4 = std::stoi("test 9821"); // 参数错误
```

* 如果字符串中所有字符都是数值类型，整个字符串会被转换为对应的数值，并通过返回值返回

* 如果字符串的前半部分字符是数值类型，后半部不是，那么前半部分会被转换为对应的数值，并通过返回值返回

* **如果字符第一个字符不是数值类型转换失败**





## 断言

断言（assertion）是一种编程中常用的手段。在通常情况下，断言就是将一个返回值总是需要为真的判断表达式放在语句中，用于排除在设计的逻辑上不应该产生的情况。

比如：一个函数总需要输入在一定的范围内的参数，那么程序员就可以对该参数使用断言，以迫使在该参数发生异常的时候程序退出，从而避免程序陷入逻辑的混乱。 

从一些意义上讲，断言并不是正常程序所必需的，不过对于程序调试来说，通常断言能够帮助程序开发者快速定位那些违反了某些前提条件的程序错误。

如果我们要在 C++ 程序中使用断言，需要在程序中包含头文件 `<cassert>` 或 `<assert.h>`，头文件中为我们提供了 `assert` 宏，**用于在运行时进行断言。**

```cpp
#include<cassert>

char* creatArray(int size) {
  // 通过断言判断数组大小是否大于0
  assert(size > 0);  // 必须大于0，否则程序中断退出
  return new char[size];
}
```



### 静态断言

assert 是一个运行时断言，也就是说它只有在程序运行时才能起作用。C++11提供了静态断言。

静态断言 `static_assert`，所谓**静态就是在编译时就能够进行检查的断言**，使用时不需要引用头文件。静态断言的另一个好处是，可以自定义违反断言时的错误提示信息。静态断言使用起来非常简单，它接收两个参数：

> 参数1：断言表达式，这个表达式通常需要返回一个 bool值
>
> 参数2：警告信息，它通常就是一段字符串，在违反断言（表达式为false）时提示该信息

```cpp
static_assert(size > 0, "error: size must greater than 0");
```

**由于静态断言的表达式是在编译阶段进行检测，所以在它的表达式中不能出现变量，也就是说这个表达式必须是常量表达式。**



## noexcept

C++98异常被抛出后，从进入 try 块起，到异常被抛掷前，这期间在栈上构造的所有对象，都会被自动析构。析构的顺序与构造的顺序相反。这一过程称为**栈的解旋**。

C++11新标准提出了`noexcept`， 表示其修饰的函数不会抛出异常 。不过与 `throw ()` 动态异常声明不同的是，**在 C++11 中如果 noexcept 修饰的函数抛出了异常，编译器可以选择直接调用 `std::terminate () `函数来终止程序的运行，这比基于异常机制的 throw () 在效率上会高一些。**

这是因为异常机制会带来一些额外开销，比如函数抛出异常，会导致函数栈被依次地展开（栈解旋），并自动调用析构函数释放栈上的所有对象。

```cpp
double myDiv(int a, int b) noexcept {
  if (b == 0) {
    cout << "div by zero!\n";
    return -1;
  }
  return a / b;
}
```

noexcept 还可以可以接受一个常量表达式（被转成bool类型）作为参数：

```cpp
double divisionMethod(int a, int b) noexcept(常量表达式);
```

* 值为 true，表示函数不会抛出异常
* 值为 false，表示有可能抛出异常这里

**不带常量表达式的 noexcept 相当于声明了 noexcept（true），即不会抛出异常。**



## 自动类型推导

在 C++11 中增加了很多新的特性，比如可以使用 `auto` 自动推导变量的类型，还能够结合`decltype `来表示函数的返回值。

### auto

C++11 中 auto 并不代表一种实际的数据类型，只是一个类型声明的 “占位符”，auto 并不是万能的在任意场景下都能够推导出变量的实际类型，使用auto声明的变量必须要进行初始化，以让编译器推导出它的实际类型，在编译时将auto占位符替换为真正的类型。

```cpp
auto t1 = 3.14;
auto* t2 = &t1;
const auto t3 = t1;
```



### auto的限制

1、不能作为函数参数使用。因为只有在函数调用的时候才会给函数参数传递实参，auto 要求必须要给修饰的变量赋值，因此二者矛盾。

```cpp
int func(auto a, auto b){}		// 错误
```

2、不能用于类的非静态成员变量的初始化

```cpp
class Base {
  auto v1 = 0;			//  error
  static auto v2 = 0;	// error，类的静态非常量成员不允许在类内部直接初始化
  static const auto v3 = 10;		// ok
};
```

3、不能使用auto定义数组

```cpp
int arr[] = {1, 2, 3, 4};
auto t3[] = {1, 2, 3, 4};	// error, auto 无法定义数组
auto t2[] = arr;			// error, auto 无法定义数组
auto t1 = arr;				// ok
```

4、无法使用auto推导出模板参数

```cpp
template <typename T>
struct Test{};

Test<double> t1;
Test<auto> t2 = t1;			// error, 无法推导出模板类型
```



### decltype

在某些情况下，不需要或者不能定义变量，但是希望得到某种类型，这时候就可以使用 C++11 提供的 decltype 关键字了，它的作用是在编译器编译的时候推导出一个表达式的类型。主要是为泛型编程而设计，以解决泛型编程中，由于有些类型由模板参数决定，而难以（甚至不可能）表示之的问题。

decltype 是 “declare type” 的缩写，意思是 “声明类型”。decltype 的推导是在编译期完成的，它只是用于表达式类型的推导，并不会计算表达式的值。

```cpp
//语法格式：	decltype (表达式)
int t1 = 10;
decltype(t1) t2 = 99;			// t2->int
decltype(t1+3.14) t3 = 6.18;	// t3->double
```

### decltype推导规则

**1、表达式为普通变量、普通表达式或者类表达式，使用decltype推导出的类型和表达式类型一致。**

```cpp
class Test {
 private:
 public:
  string text;
  const static int value = 110;
};

int main(int argc, char const* argv[]) {
  int t1 = 99;
  const int& t2 = t1;
  decltype(t1) a = t1;          // a => int
  decltype(t2) b = t1;          // b => const int&
  decltype(Test::value) c = 0;  // c => const int

  Test t;
  decltype(t.text) d = "Hello, world!";  // d => string

  return 0;
}
```



**2、表达式是函数调用，decltype推导出的类型和函数返回值一致。**

```cpp
int func01();
int& func02();
const int func03();
const int& func04();

int t1 = 100;
decltype(func01()) a = 0;   // a => int
decltype(func02()) b = t1;  // b => int&
decltype(func03()) c = 0;   // c => int
decltype(func04()) d = 0;	// d => const int&
```

函数 `func03()` 返回的是一个纯右值（在表达式执行结束后不再存在的数据，也就是临时性的数据），**对于纯右值而言，只有类类型可以携带const、volatile限定符**，除此之外需要忽略掉这两个限定符，因此推导出的变量 d 的类型为 `int` 而不是 `const int`。



**3、表达式是一个左值，或者被括号`( )`包围，则decltype推导出的是表达是类型的引用（如果有const、volatile限定符不能省略）。**

```cpp
int main(int argc, char const* argv[]) {
  int a = 10;
  decltype((a)) b = a;  // 带有括号的表达式，所以b的类型是 int&
  cout << &a << endl;   // a和b的地址是一样的
  cout << &b << endl;

  int x = 0;
  decltype(a += 10) c = x;  // a是一个左值，所以c的类型是 int&
  cout << &c << endl;       // c和x的地址是一样的
  cout << &x << endl;

  return 0;
}
```



### decltype的应用

关于 decltype 的应用多出现在泛型编程中。

```cpp
template <class T>
class Container {  // 要求T必须是一个容器类型
 private:
  // T()使用默认构造函数创建一个临时对象，T().begin()调用临时对象的begin成员函数
  decltype(T().begin()) m_it;  // 迭代器的类型使用decltype确定
 public:
  void func(T& c) {
    for (m_it = c.begin(); m_it != c.end(); ++m_it) {
      cout << *m_it << " ";
    }
    cout << endl;
  }
};

int main() {
  const list<int> lst{1, 2, 3, 4, 5, 6, 7, 8, 9};
  Container<const list<int>> obj;
  obj.func(lst);
  return 0;
}
```



### 返回类型后置

在C++11中增加了返回类型后置语法，**说明白一点就是将decltype和auto结合起来完成返回类型的推导，auto 会追踪 decltype() 推导出的类型**。其语法格式如下:

```cpp
auto func(参数1, 参数2, ...) -> decltype(参数表达式) {}
```

```cpp
int& func(int& a) { return a += 10; }

double func(double& a) { return a += 20.0; }

template <typename T>
// decltype里面的表达式不唯一，只要能推导出 类型即可
auto test(T& t) -> decltype(func(t)) {		// 返回值类型后置
  return func(t);
}

int main() {
  int x = 10;
  double y = 20;

  auto a = test<int>(x);
  auto b = test<double>(y);

  /* 对于函数模板，只要编译器可以自动推导出类型，则不需要加尖括号 */
  auto c = test(x);
  auto d = test(y);

  return 0;
}

```





## 基于范围的 for 循环

C++11中for语句的能力被进一步扩展，引入了范围for语句，用于遍历一个序列

```cpp
int arr[] = {1, 2, 3, 4};
for(auto x : arr) {
    printf("%d", x);
}
for(auto x : {1, 2, 3, 4}) {
    printf("%d", x);
}
```

上面的的for语句的写法有个缺点，多了一个复制的动作，也就是把数组arr中的元素依次复制到了x中，可以利用引用进行优化

```cpp
int arr[] = {1, 2, 3, 4};
// 遍历打印元素
for(const auto &x : arr) {
    printf("%d", x);
}

// 可以修改元素的值，但不能插入或删除元素，改变容器大小
for(auto &x : arr) {
    printf("%d", ++x);
}
```

一般来讲，一个容器只要其内部支持begin和end成员函数用于返回一个迭代器，能够指向容器的第一个元素和末端元素的后面，这种容器就可以支持范围for语句。

> 对于范围for语句中，不要在里边改变容器的容量，增加或者删除元素等。否则会导致输出混乱。可以修改元素自身的值（set容器除外）。

使用基于访问的 for 循环遍历关联性容器，auto 自动推导出的类型是容器中的 value_type，相当于一个对组（`std::pair`）对象。

```cpp
std::map<int, string> m{{1, "Lucy"}, {2, "Nancy"}, {3, "Rocky"}};

for (const auto& x : m) {
    cout << "id: " << x.first << ", name: " << x.second << endl;
}
```



### 元素只读

对应 `set ` 容器来说，内部元素都是只读的，这是由容器的特性决定的，因此在 for 循环中 `auto &` 会被视为 `const auto &` 。

除此之外，在遍历关联型容器时也会出现同样的问题，基于范围的for循环中，虽然可以得到一个std::pair引用，**但是我们是不能修改里边的first值的，也就是key值。**

### 访问次数

基于范围的 for 循环遍历的对象可以是一个表达式或者容器 / 数组等。假设我们对一个容器进行遍历，在遍历过程中 for 循环对这个容器只访问一次。

```cpp
vector<int> v{1, 2, 3, 4, 5};
vector<int>& getVect() {
  cout << "Get vector...\n";
  return v;
}

int main(int argc, char const* argv[]) {
  for (const auto x : getVect()) {
    printf("%d ", x);
  }
  return 0;
}
```

不论基于范围的 for 循环迭代了多少次，函数 `getVect() `只在第一次迭代之前被调用，得到这个容器对象之后就不会再去重新获取这个对象了。



## 指针空类型 nullptr

nullptr 是C++11引入的新关键字，代表“空指针”。nullptr 专用于初始化空类型指针，不同类型的指针变量都可以使用 nullptr 来初始化。

```cpp
int*    ptr1 = nullptr;
char*   ptr2 = nullptr;
double* ptr3 = nullptr;

char* p = NULL;	
char* q = nullptr;

if(p == nullptr) {...}		// 条件能成立
```

```cpp
cout << typeid(NULL).name() << endl;
cout << typeid(nullptr).name() << endl;
```

NULL和nullptr是两种不同的类型，以后对于指针的初始化尽可能使用nullptr代替NULL。某些方面可以避免整数和指针之间的混淆。

nullptr 无法隐式转换为整形，但是可以隐式匹配指针类型。在 C++11 标准下，相比 NULL 和 0，使用 nullptr 初始化空指针可以令我们编写的程序更加健壮。



## lambda表达式

lambda 表达式是 C++11 最重要也是最常用的特性之一，这是现代编程语言的一个特点，lambda 表达式有如下的一些优点：

* 声明式的编程风格：就地匿名定义目标函数或函数对象，不需要额外写一个命名函数或函数对象。

* 简洁：避免了代码膨胀和功能分散，让开发更加高效。

* 在需要的时间和地点实现功能闭包，使程序更加灵活。

lambda 表达式定义了一个匿名函数，并且可以捕获一定范围内的变量。lambda 表达式的语法形式简单归纳如下：

```cpp
[capture](params) opt -> ret {body;};
```

其中 capture 是捕获列表，params 是参数列表，opt 是函数选项，ret 是返回值类型，body 是函数体。

* 捕获列表 []: 捕获一定范围内的变量
* 参数列表 (): 和普通函数的参数列表一样，如果没有参数参数列表可以省略不写。

```cpp
auto f = [](){return 1;}	// 没有参数, 参数列表为空
auto f = []{return 1;}		// 没有参数, 参数列表省略不写
```

* opt 选项， 不需要可以省略

  > mutable: 可以修改按值传递进来的拷贝（注意是能修改拷贝，而不是值本身）
  > exception: 指定函数抛出的异常，如抛出整数类型的异常，可以使用 throw ();

* 返回值类型：在 C++11 中，lambda 表达式的返回值是通过返回值后置语法来定义的。

* 函数体：函数的实现，这部分不能省略，但函数体可以为空。

### 捕获列表

lambda 表达式的捕获列表可以捕获一定范围内的变量，具体使用方式如下：

- `[] ` - 不捕捉任何变量
- `[&] `- 捕获外部作用域中所有变量，并作为引用在函数体内使用 (按引用捕获)
- `[=]` - 捕获外部作用域中所有变量，并作为副本在函数体内使用 (按值捕获)。**拷贝的副本在匿名函数体内部是只读的**
- `[=, &foo]` - 按值捕获外部作用域中所有变量，并按照引用捕获外部变量 foo
- `[bar]` - 按值捕获 bar 变量，同时不捕获其他变量
- `[&bar]` - 按引用捕获 bar 变量，同时不捕获其他变量
- `[this]` - 捕获当前类中的 this 指针。让 lambda 表达式拥有和当前类成员函数同样的访问权限。如果已经使用了 & 或者 =, 默认添加此选项

```cpp
class Human {
 private:
  int age;

 public:
  void func(int a, int b) {
    // 按值捕获外部作用域中所有变量
    auto t2 = [=]() { return a + b + age; };

    //按引用捕获外部作用域中所有变量
    auto t3 = [&]() { return a + b + age; };

    // error, 捕获 this 指针，但没有捕获到变量a,b
    auto t5 = [this]() { return a + b + age; };

    // ok, 捕获 this 指针，a, b
    auto t6 = [this, a, b]() { return a + b + age; };

    // ok, 捕获 this 指针，访问并修改对象内部变量的值
    auto t7 = [this]() { return age++; };
  }
};

int main(int argc, char const *argv[]) {
  int a = 10, b = 20;
  // ok, 按引用捕获外部变量，可读写
  auto t1 = [&]() { return a++; };

  // error, 按值捕获外部变量，可读不能写
  auto t2 = [=]() { return a++; };

  // ok, 按引用捕获b, 其他所有按值捕获
  auto t3 = [=, &b]() { return a + (b++); };

  return 0;
}
```

> 在匿名函数内部，需要通过 lambda 表达式的捕获列表控制如何捕获外部变量，以及访问哪些变量。默认状态下 lambda 表达式无法修改通过按值捕获的外部变量，**如果希望修改这些外部变量，需要通过引用的方式进行捕获。**

### 返回值

一般情况下，不指定 lambda 表达式的返回值，编译器会根据 return 语句自动推导返回值的类型，但需要注意的是 labmda表达式不能通过列表初始化自动推导出返回值类型。

```cpp
// error，不能推导出返回值类型
auto f1 = []() {
  return {1, 2};	// 基于列表初始化推导返回值，错误
}
```



### 函数本质

使用 lambda 表达式捕获列表捕获外部变量，如果希望去修改按值捕获的外部变量，那么应该如何处理呢？这就需要使用 mutable 选项，被mutable修改是lambda表达式就算没有参数也要写明参数列表，并且可以去掉按值捕获的外部变量的只读（const）属性。

```cpp
int a = 10;
auto f2 = [=]()mutable {return a++; };     // ok, 虽然按值捕获，但是可以修改值
```

最后再剖析一下为什么通过值拷贝的方式捕获的外部变量是只读的:

> lambda表达式的类型在C++11中会被看做是一个带operator()的类，即仿函数。按照C++标准，lambda表达式的operator()默认是const的，一个const成员函数是无法修改成员变量值的。**mutable 选项的作用就在于取消 operator () 的 const 属性。**

因为 lambda 表达式在 C++ 中会被看做是一个仿函数，因此可以使用std::function和std::bind来存储和操作lambda表达式：

```cpp
int main(int argc, char const *argv[]) {
  // 包装可调用函数
  std::function<int(int)> f1 = [](int a) { return a; };

  // 绑定可调用函数
  std::function<int(int)> f2 =
      std::bind([](int a) { return a; }, std::placeholders::_1);

  cout << f1(100) << endl;
  cout << f2(200) << endl;
  return 0;
}
```

对于没有捕获任何变量的 lambda 表达式，还可以转换成一个普通的函数指针：

```cpp
// 定义函数指针
using func_ptr = int (*)(int);

// 没有捕获任何变量的Lambda表达式可以转换成一个函数指针
func_ptr func = [](int a) { return a; };

func(100);
```



## constexpr

### const

在 C++11 之前只有 const 关键字，从功能上来说这个关键字有双重语义：变量只读，修饰常量。**变量只读不等价于常量**

```cpp
void func(const int a) {
  const int n = 100;
  int arr1[a];  // error, a是一个只读变量，不是常量
  int arr2[n];  // ok, n是一个常量

  int t1 = 98;
  int t2 = 21;
  const int& b = t1;

  // error, b是一个常量引用，不是常量，是只读的，不能保证变量的值不能改变
  b = 200;

  // ok, b的值现在变为200
  t1 = 200;
}
```



### constexpr

在 C++11 中添加了一个新的关键字 `constexpr`，这个关键字是用来修饰常量表达式的。**所谓常量表达式，指的就是由多个（≥1）常量（值不会改变）组成并且在编译过程中就得到计算结果的表达式。**

在介绍 gcc/g++ 工作流程的时候说过，C++ 程序从编写完毕到执行分为四个阶段：**预处理、 编译、汇编和链接 4 个阶段**，得到可执行程序之后就可以运行了。

常量表达式和非常量表达式的计算时机不同，非常量表达式只能在程序运行阶段计算出结果，**但是常量表达式的计算往往发生在程序的编译阶段，这可以极大提高程序的执行效率**，因为表达式只需要在编译阶段计算一次，节省了每次程序运行时都需要计算一次的时间。

那么问题来了，编译器如何识别表达式是不是常量表达式呢？在 C++11 中添加了 constexpr 关键字之后就可以在程序中使用它来修改常量表达式，用来提高程序的执行效率。**在使用中建议将 const 和 constexpr 的功能区分开，即凡是表达“只读”语义的场景都使用 const，表达“常量”语义的场景都使用 constexpr。**

```cpp
// 在定义常量时，const 和 constexpr 是等价的
const int i = 98;			// 是一个常量表达式
constexpr int j = 21;		// 是一个常量表达式
```

> 对于 C++ 内置类型的数据，可以直接用 constexpr 修饰，但如果是自定义的数据类型（用 struct 或者 class 实现），直接用 constexpr 修饰是不行的。



### 常量表达式函数

了提高 C++ 程序的执行效率，我们可以将程序中值不需要发生变化的变量定义为常量，也可以使用 constexpr 修饰函数的返回值，这种函数被称作**常量表达式函数**，这些函数主要包括以下几种：**普通函数/类成员函数、类的构造函数、模板函数。**



### 修饰普通或者成员函数

必须满足以下条件：

1、函数必须要有返回值，并且 return 返回的表达式必须是常量表达式。

2、函数在使用之前，必须有对应的定义语句。

3、整个函数的函数体中，不能出现非常量表达式之外的语句（using 指令、typedef 语句以及 static_assert 断言、return 语句除外）。

```cpp
// error
constexpr void func1() {
  for (int i = 0; i < 10; ++i) {
    // cout 要在运行时才知道
    cout << "i: " << i << endl;
  }
}

// ok, 可以使用using
constexpr int func2() {
  using mytype = int;
  constexpr mytype a = 100;
  constexpr mytype b = 10;
  return a + b;
}
```

constexpr 隐含有`inline`属性，所以用constexpr修饰函数，函数应该尽可能简单。如果使用递归，则递归不要太深。

```cpp
constexpr uint64_t fact(uint64_t n) {
  if (n == 1) {
    return n;
  }
  return n * fact(n - 1);
}

int main(int argc, char const *argv[]) {
  // 加上constexpr，告诉编译器在编译时期确定结果
  // 种瓜得瓜种豆得豆，如果传入的是确定的常量，则编译时能计算出来
  constexpr auto v1 = fact(10);
  // 如果传入的是未确定的变量，则不能加constexpr, 只能运行时计算
  auto v2 = fact(argc);

  return 0;
}
```



### 修饰模板函数

C++11 语法中，constexpr 可以修饰函数模板，但由于模板中类型的不确定性，因此函数模板实例化后的模板函数是否符合常量表达式函数的要求也是不确定的。**如果 constexpr 修饰的模板函数实例化结果不满足常量表达式函数的要求，则 constexpr 会被自动忽略，即该函数就等同于一个普通函数。**

```cpp

typedef struct Person {
  const char* name;
  int age;
} Person;

// 定义函数模板
template <typename T>
constexpr T display(T t) {
  return t;
}

int main(int argc, char const* argv[]) {
  Person p{"lucky", 19};
  // 普通函数, 由于参数 p 是变量，所以实例化后的函数不是常量表达式函数，此时 constexpr 是无效的
  Person p1 = display(p);
  cout << "name: " << p1.name << ", age: " << p1.age << endl;

  // 常量表达式函数，参数是常量，符合常量表达式函数的要求，此时 constexpr 是有效的
  constexpr int t = display(250);
  cout << t << endl;
    
  // 参数是常量，符合常量表达式函数的要求，此时 constexpr 是有效的
  constexpr Person p2{"Rocky", 19};
  constexpr Person p3 = display(p2);
  cout << "name: " << p3.name << ", age: " << p3.age << endl;

  return 0;
}
```

###  修饰构造函数

如果想用直接得到一个常量对象，也可以使用 constexpr 修饰一个构造函数，这样就可以得到一个常量构造函数了。**常量构造函数有一个要求：构造函数的函数体必须为空，并且必须采用初始化列表的方式为各个成员赋值。**

```cpp
class Person {
 public:
  const char* name;
  int age;

 public:
  // 常量构造函数，使用初始化列表
  constexpr Person(const char* name, int age) : name(name), age(age) {}
};

int main(int argc, char const* argv[]) {
  constexpr Person p{"lucky", 19};
  cout << "name: " << p.name << ", age: " << p.age << endl;

  return 0;
}
```



## 委托构造和继承构造函数

### 委托构造函数

```cpp
class Father {
 public:
  string name;
  int age;
  int weight;

 public:
  Father();
  Father(string name) : name(name), age(0), weight(0) {}
  // 使用委托构造函数，可避免代码冗余
  Father(string name, int age) : Father(name) { age = age; }
  Father(string name, int age, int weight) : Father(name, age) {
    weight = weight;
  }

  void func(string name) { cout << "Father name: " << name << endl; }
  void func(string name, int age) {
    cout << "Father name: " << name << ", age: " << age << endl;
  }
};
```

**这种链式的构造函数调用不能形成一个闭环（死循环），否则会在运行期抛异常。**

如果要进行多层构造函数的链式调用，建议将构造函数的调用的写在初始列表中，而且其他成员变量的初始化必须在函数体中。

```cpp
// error, 调用Person(name)后，不能再跟其他参数
Person(string name, int age) : Person(name), age(age){}
```



### 继承构造函数

继承构造函数的使用方法是这样的：通过使用 `using 类名::构造函数名`（其实类名和构造函数名是一样的）来声明使用基类的构造函数，这样子类中就可以不定义相同的构造函数了，直接使用基类的构造函数来构造派生类对象。

```cpp
class Son : public Father {
 public:
  // 继承父类的构造函数
  using Father::Father;
  // 继承父类的func函数，使得父类的func函数不再被隐藏
  using Father::func;

  // 定义子类的func函数
  void func() { cout << "Son!!!" << endl; }
};

int main(int argc, char const* argv[]) {
  Son son("lucky");
  son.func();             // 调用子类的func函数
  son.func("Rocky", 19);  // 调用父类的func函数

  return 0;
}
```

子类中的 `func()` 函数隐藏了基类中的两个 func() 因此默认情况下通过子类对象只能调用无参的` func()`，在上面的子类代码中添加了 `using Father::func;` 之后，就可以通过子类对象直接调用父类中被隐藏的带参 `func() `函数了。



## 右值引用

你不知道你，所以你是你。如果你知道你，你就不是你。右值描述的是一种没有主人，没有名字，不能取地址的一个值或者对象，它在内存某个区域临时存储着。一旦你用一个变量绑定它，也就是给它起个别名，那么它就不再是右值而变成左值，因为它可以取地址了。

### 左值和右值

C++11 增加了一个新的类型，称为右值引用（ R-value reference），标记为 `&&`。

`lvalue `是 `loactor value `的缩写，`rvalue `是 `read value `的缩写

* 左值是指存储在内存中、有明确存储地址（可取地址）的数据；

* 右值是指可以提供数据值的数据（不可取地址），比如字面量，临时对象；

区分左值与右值的便捷方法是：**可以对表达式取地址（&）就是左值，否则为右值 。**所有有名字的变量或对象都是左值，而右值是匿名的。

C++11 中右值可以分为两种：一个是将亡值（ xvalue, expiring value），另一个则是纯右值（ prvalue, PureRvalue）：

* 纯右值：非引用返回的临时变量、运算表达式产生的临时变量、原始字面量和 lambda 表达式等

* 将亡值：与右值引用相关的表达式，比如，T&& 类型函数的返回值、 std::move 的返回值等。

> `++i`是直接给变量i加1，然后返回i本身，因为i是变量，所以可以被赋值，因此是左值表达式。
>
> `i++`先产生一个临时变量来保存i的值，用于使用目的，再给i加1，接着返回临时变量，之后系统再释放这个临时变量。临时变量被释放掉了，不能再被赋值，因此是右值表达式。



### 右值引用

右值引用就是对一个右值进行引用的类型。**因为右值是匿名的，所以我们只能通过引用的方式找到它。**无论声明左值引用还是右值引用都必须立即进行初始化，因为引用类型本身并不拥有所绑定对象的内存，只是该对象的一个别名。**通过右值引用的声明，该右值又“重获新生”，其生命周期与右值引用类型变量的生命周期一样，只要该变量还活着，该右值临时量将会一直存活下去。**

> 左值代表一个地址、一个变量的感觉。一般来讲，左值是一个持久的值，右值是一个短暂的值。右值要么是字面值常量，要么就是一个表达式求值过程中创建的临时对象，这个临时对象的特性为：
>
> * 所引用的对象将要被销毁
> * 该对象没有其他用户
>
> 所以右值引用能自由地接管所引用的对象资源。
>

```cpp
int i = 1;
int&& ri = i++;			// 将r1绑定到系统返回的临时变量，这时临时变量就不会再被销毁了
```

左值引用（绑定到左值）：引用那些希望改变值的对象，带一个`&`

const引用：也是左值引用，但是引用那些不希望改变值的对象，如常量等。常量引用也可以绑定到右值上，所以比较特殊。称为“万能引用”。

右值引用（绑定到右值）：C++11新概念。首先它也是一个引用，但侧重表示引用对象的值在使用之后就无需保留了（如临时变量），带两个`&`。主要用来绑定到那些“即将销毁/临时的对象”上。

```cpp
int value = 10;

int& lvalue = value;  // 左值引用，绑定到一个左值，为左值起个别名

const int& cvalue = value;  // 常量引用，也是左值引用，不能通过cvalue修改value的值
const int& cvalue2 = 10;  // 常量引用也可以绑定到右值上，所以比较特殊

int&& rvalue = 10;  // 右值引用，绑定到右值，也就是一个常量
rvalue = 20;        // 右值引用可以修改值
```

### 临时对象

减少临时对象的产生，可以提高程序的性能和效率。产生临时对象的情况有以下几种：

1. 以传值的方式给函数传递参数
2. 类型转换生成的临时对象/隐式类型转换以保证函数调用成功
3. 函数返回对象的时候
4. 类外的运算符重载





### 性能优化

在 C++ 中在进行对象赋值操作的时候，很多情况下会发生对象之间的深拷贝，如果堆内存很大，这个拷贝的代价也就非常大，在某些情况下，如果想要避免对象的深拷贝，就可以使用右值引用进行性能的优化。

右值引用是C++11引入的新概念，目的是提高程序运行效率问题，提高的手段是把复制对象变成移动对象。

右值引用具有移动语义，移动语义可以将资源（堆、系统对象等）通过浅拷贝从一个对象转移到另一个对象这样就能减少不必要的临时对象的创建、拷贝以及销毁，可以大幅提高 C++ 应用程序的性能。

```cpp
class Base {
 private:
  int* ptr;

 public:
  Base() : ptr(new int(100)) { cout << "Construct function" << endl; }

  // 拷贝构造函数
  Base(const Base& a) : ptr(new int(*a.ptr)) {
    cout << "Copy construct function" << endl;
  }

  // 移动构造函数
  Base(Base&& a) : ptr(a.ptr) {
    a.ptr = nullptr;
    cout << "Move construct" << endl;
  }

  ~Base() {
    cout << "Destruct function" << endl;

    delete ptr;
  }

 public:
};

//getter() 的返回值就是一个将亡值，也就是说是一个右值。
//在进行赋值操作的时候如果 = 右边是一个右值，那么移动构造函数就会被调用。
Base getter() {
  Base b;
  // 会返回一个临时对象
  return b;
}

int main(int argc, char const* argv[]) {
  // 会调用移动构造函数，将b绑定到getter返回的临时对象上，避免了深拷贝
  Base b = getter();

  return 0;
}
```

> 对于需要动态申请大量资源的类，应该设计移动构造函数，以提高程序效率。需要注意的是，我们一般在提供移动构造函数的同时，也会提供常量左值引用的拷贝构造函数，以保证移动不成还可以使用拷贝构造函数。

### 移动构造函数

C++规定：移动构造函数第一个参数就是一个右值引用参数，**C++根据传递进来的是否是一个右值实参来确定是否要调用移动构造函数或者移动赋值运算符。**

移动构造函数第一个参数是右值引用，如果有其他额外参数，那么这些参数必须有默认值。

（1）一般来讲，如果类使用使用new分配了大量内存，则需要使用移动构造函数和移动赋值运算符。

（2）不抛出异常的移动构造函数、移动赋值运算符都应该加上noexcept，用于通知编译器该函数本身不抛出异常，还可以提高编译效率。

（3）一个对象被移动完数据后，程序员有责任将使该对象处于可以被释放的状态。



### &&的特性

在 C++ 中，并不是所有情况下 && 都代表是一个右值引用，具体的场景体现在模板和自动类型推导中，如果是模板参数需要指定为 T&&，如果是自动类型推导需要指定为 auto &&，**在这两种场景下 && 被称作未定的引用类型。**另外还有一点需要额外注意` const T&&` 表示一个右值引用，不是未定引用类型。

```cpp
template <typename T>
void func01(T&& p);

template <typename T>
void func02(const T&& p);

int main(int argc, char const* argv[]) {
  int x = 10;

  func01(10);  // T&& 表示右值引用
  func01(x);   // T&& 表示右值引用

  func02(10);  // const T&& 表示右值引用，需要传入右值参数
  return 0;
}
```



```cpp
int x = 98, y = 21;
auto&& t1 = x;   // auto&& 表示一个整型的左值引用
auto&& t2 = 10;  // auto&& 表示一个整形的右值引用

// error,  decltype(x)&& 表示int&&，不能用左值去初始化右值
decltype(x)&& t3 = y;
```

由于上述代码中存在 T&& 或者 auto&& 这种未定引用类型，当它作为参数时，有可能被一个右值引用初始化，也有可能被一个左值引用初始化，**在进行类型推导时右值引用类型（&&）会发生变化，这种变化被称为引用折叠。**在 C++11 中引用折叠的规则如下：

* **通过右值推导 T&& 或者 auto&& 得到的是一个右值引用类型**

* 通过非右值（右值引用、左值、左值引用、常量右值引用、常量左值引用）推导 T&& 或者 auto&& 得到的是一个左值引用类型

```cpp
int&& t1 = 5;
auto&& t2 = t1;  // t1为右值引用，推导出的 auto&& 为左值引用
auto&& t3 = 5;   // 5为右值，推导出的 auto&& 为右值引用

int v1 = 5;
int& v2 = v1;
auto&& v3 = v2;  // v2为左值引用，推导出的auto&& 为左值引用
auto&& v4 = v1;  // a2为左值，推导出的auto&& 为左值引用

const int& s1 = 100;
const int&& s2 = 100;
auto&& s3 = s1;  // s1位常量左值引用，推导出的auto&& 为常量左值引用
auto&& s4 = s2;  // s2位常量右值引用，推导出的auto&& 为常量左值引用

const auto&& x = 5;  // x为右值引用，不需要推导，只能通过右值初始化
```



```cpp
void printValue(int& a) { cout << "l-value: " << a << endl; }

void printValue(int&& a) { cout << "r-value: " << a << endl; }

void forward(int&& a) { printValue(a); }

int main(int argc, char const* argv[]) {
  int t = 98;
  printValue(t);   // 左值 l-value
  printValue(21);  // 右值 r-value

  //一个右值引用作为函数参数的形参时，在函数内部转发该参数给内部其他函数时，它就变成一个左值，并不是原来的类型了
  forward(100);

  return 0;
}
```

最后总结一下关于 && 的使用：

- 左值和右值是独立于他们的类型的，右值引用类型可能是左值也可能是右值。
- **编译器会将已命名的右值引用视为左值，将未命名的右值引用视为右值。**
- auto&&或者函数参数类型自动推导的T&&是一个未定的引用类型，它可能是左值引用也可能是右值引用类型，这取决于初始化的值类型（上面有例子）。
- **通过右值推导 T&& 或者 auto&& 得到的是一个右值引用类型，其余都是左值引用类型。**



## 转移和完美转发

### std::move()

在 C++11 添加了右值引用，并且不能使用左值初始化右值引用，如果想要使用左值初始化一个右值引用需要借助 std::move () 函数，**使用std::move方法可以将左值转换为右值。**使用这个函数并不能移动任何东西，**而是和移动构造函数一样都具有移动语义，**将对象的状态或者所有权从一个对象转移到另一个对象，**只是转移，没有内存拷贝。**

从实现上讲，std::move 基本等同于一个类型转换：static_cast<T&&>(lvalue);，函数原型如下:

```cpp
template<class _Ty>
 // forward _Arg as movable
_NODISCARD constexpr remove_reference_t<_Ty>&& move(_Ty&& _Arg) _NOEXCEPT {	
    return (static_cast<remove_reference_t<_Ty>&&>(_Arg));
}
```

```cpp
int t1 = 98;
printf("%x\n", &t1);		// t1和t2地址相同

int&& t2 = std::move(t1);  // t1是一个左值，move(t1)为右值
printf("%x\n", &t2);
```

假设一个临时容器很大，并且需要将这个容器赋值给另一个容器，就可以执行如下操作：

```cpp
list<string> ls;
ls.push_back("hello");
ls.push_back("world");
......
list<string> ls1 = ls;        // 需要拷贝, 效率低
list<string> ls2 = move(ls);
```

如果不使用 `std::move`，拷贝的代价很大，性能较低。**使用 move 几乎没有任何代价，只是转换了资源的所有权。**

如果一个对象内部有较大的堆内存或者动态数组时，使用 move () 就可以非常方便的进行数据所有权的转移。

另外，我们也可以给类编写相应的**移动构造函数** `T::T(T&& a)` 和**具有移动语义的赋值函数**`T&& T::operator=(T&& a)`，在构造对象和赋值的时候尽可能的进行资源的重复利用，因为它们都是接收一个右值引用参数。



### forward

右值引用类型是独立于值的，**一个右值引用作为函数参数的形参时，在函数内部转发该参数给内部其他函数时，它就变成一个左值，并不是原来的类型了。**如果需要按照参数原来的类型转发到另一个函数，可以使用 C++11 提供的 `std::forward () `函数，该函数实现的功能称之为**完美转发**。

```cpp
// 函数原型
template <class T> T&& forward (typename remove_reference<T>::type& t) noexcept;
template <class T> T&& forward (typename remove_reference<T>::type&& t) noexcept;

// 精简之后的样子
std::forward<T>(t);
```

* 当T为左值引用类型时，t将被转换为T类型的左值
* 当T不是左值引用类型时，t将被转换为T类型的右值

```cpp
template <typename T>
void printValue(T& t) {
  cout << "l-value: " << t << endl;
}

template <typename T>
void printValue(T&& t) {
  cout << "r-value: " << t << endl;
}

template <typename T>
void testForward(T&& v) {
  printValue(v);
  printValue(std::move(v));
  printValue(std::forward<T>(v));

  cout << endl;
}

int main(int argc, char const* argv[]) {
  int t = 21;
  testForward(98);
  testForward(t);
  testForward(std::forward<int>(t));
  testForward(std::forward<int&>(t));
  testForward(std::forward<int&&>(t));

  return 0;
}
```



`testForward(98); `函数的形参为未定引用类型 T&&，实参为右值，初始化后被推导为一个右值引用

- `printValue(v); `已命名的右值 v，编译器会视为左值处理，实参为左值
- `printValue(move(v)); `已命名的右值编译器会视为左值处理，通过 move 又将其转换为右值，实参为右值
- `printValue(forward<T>(v)); `forward 的模板参数为右值引用，最终得到一个右值，实参为 右值

`testForward(t); `函数的形参为未定引用类型 T&&，实参为左值，初始化后被推导为一个左值引用

- `printValue(v); `实参为左值
- `printValue(move(v)); `通过 move 将左值转换为右值，实参为右值
- `printValue(forward<T>(v));` forward 的模板参数为左值引用，最终得到一个左值，实参为左值

`testForward(forward<int>(t)); `forward 的模板类型为 int，最终会得到一个右值，函数的形参为未定引用类型 T&& 被右值初始化后得到一个右值引用类型。所以结果同`testForward(98);`

`testForward(forward<int&>(t)); `forward 的模板类型为 int&，最终会得到一个左值，函数的形参为未定引用类型 T&& 被左值初始化后得到一个左值引用类型。所以结果通`testForward(t);`

`testForward(forward<int&&>(t));` forward 的模板类型为 int&&，最终会得到一个右值，函数的形参为未定引用类型 T&& 被右值初始化后得到一个右值引用类型。所以结果同`testForward(98);`



## 列表初始化

关于 C++ 中的变量，数组，对象等都有不同的初始化方法，在这些繁琐的初始化方法中没有任何一种方式适用于所有的情况。为了统一初始化方式，并且让初始化行为具有确定的效果，在 C++11 中提出了列表初始化的概念。

```cpp
class Person {
 public:
  Person(int id, string name) {
    cout << "id: " << id << ", name: " << name << endl;
  }
};

Person func() {
  // 返回一个匿名对象
  return {9527, "华安"};
}

int main(int argc, char const* argv[]) {
  int t1 = int{10};
  int t2{10};
  int* t3 = new int{98};
  int* arr1 = new int[3]{1, 2, 3};
  int arr2[]{1, 2, 3};

  Person p = func();

  return 0;
}
```



### 聚合体

使用列表初始化对对象初始化时，还需要判断这个对象对应的类型是不是一个聚合体。如果是，初始化列表中的数据就会拷贝到对象中。

```cpp
struct Test01 {
  int x, y;
} test01 = {12, 34};

struct Test02 {
  int x, y;
  Test02(int, int) : x(10), y(20) {}
} test02 = {12, 34};

int main(int argc, char const *argv[]) {
  // x:12, y: 34
  // test01是一个自定义的聚合类型，所以可以通过初始化列表初始化
  printf("test01.x: %d, test01.y: %d \n", test01.x, test01.y);

  // x:10, y: 20
  // 由于自定义一个构造函数，因此实际的初始化时通过这个构造函数完成的
  printf("test02.x: %d, test02.y: %d \n", test02.x, test02.y);
  return 0;
}
```

普通数组本身可以看做是一个聚合类型

满足以下条件的类（class、struct、union）可以被看做是一个聚合类型：

无用户自定义的构造函数、无基类、无虚函数。

无私有或保护的非静态数据成员。

```cpp
class T2 {
 public:  // 类中有私有成员，无法使用列表初始化进行初始化
  int x;
  long y;
  // 类中有非静态成员可以通过列表初始化进行初始化，但它不能初始化静态成员变量。
 protected:
  static int z;
} t{1, 100};

// 静态变量 z 不能使用列表初始化进行初始化，它的初始化遵循静态成员的初始化方式。
int T2::z = 2;
```

类中不能有使用 {} 和 = 直接初始化的非静态数据成员（从 c++14 开始就支持了）。

```cpp
class T3 {
 public:
  int x;
  double y = 1.34;
  int z[3]{1, 2, 3};
};

int main(void) {
  T3 t{520, 13.14, {6, 7, 8}};  // error, c++11不支持,从c++14开始就支持了
  return 0;
}
```



### 非聚合体

对于聚合类型的类可以直接使用列表初始化进行对象的初始化，如果不满足聚合条件还想使用列表初始化其实也是可以的，需要在类的内部自定义一个构造函数, 在构造函数中使用初始化列表对类成员变量进行初始化:

聚合类型的定义并非递归的，也就是说当一个类的非静态成员是非聚合类型时，这个类也可能是聚合类型。

```cpp
class T1 {
 private:
  int x;

 public:
  int y;

  // 构造函数
  T1(int a, int b) : x(a), y(b) { cout << "T1 construct function!" << endl; }
};

class T2 {
 public:
  T1 t1;
  int z;
};

int main(void) {
  // 由于 T1 包含私有成员，所以 T1 是非聚合类型。{1,2}调用的是 T1 的构造函数
  // 3 则是直接赋值 t2 的 z
  T2 t2{{1, 2}, 3};
  return 0;
}

```

尽管 T2 有一个非聚合类型的非静态成员 t1，T2 依然是一个聚合类型，可以直接使用列表初始化的方式进行初始化。

> 对于一个聚合类型，使用列表初始化相当于对其中的每个元素分别赋值。
>
> 而对于非聚合类型，则需要先自定义一个合适的构造函数，此时使用列表初始化将会调用它对应的构造函数。



### std::initializer_list

在 C++ 的 STL 容器中，可以**进行任意长度的数据的初始化**，使用初始化列表也只能进行固定参数的初始化，如果想要做到和 STL 一样有任意长度初始化的能力，可以使用 std::initializer_list 这个轻量级的类模板来实现。

先来介绍一下这个类模板的一些特点：

- 它是一个轻量级的容器类型，内部定义了迭代器 iterator 等容器必须的概念，遍历时得到的迭代器是只读的。
- 对于 `std::initializer_list<T>` 而言，它可以接收任意长度的初始化列表，**但是要求元素必须是同种类型 T**
- 在` std::initializer_list `内部有三个成员接口：`size(), begin(), end()。`
- `std::initializer_list `对象只能被整体初始化或者赋值。

**如果想要自定义一个函数并且接收任意个数的参数（变参函数）**，只需要将函数参数指定为 `std::initializer_list`，使用初始化列表` { } `作为实参进行数据传递即可。

```cpp
void show(const std::initializer_list<int>& a) {
  printf("size: %d\n", a.size());
  for (const auto& x : a) {
    printf("%d ", x);
  }
  printf("\n");
}

int main(int argc, char const* argv[]) {
  std::initializer_list<int> ilist = {1, 2, 3, 4, 5, 6, 7, 8};
  show(ilist);

  // 直接通过初始化列表传递数据
  show({2, 4, 6, 8});
}
```

`std::initializer_list`，因为在遍历这种类型的容器的时候得到的是一个只读的迭代器，因此我们不能修改里边的数据，只能通过值覆盖的方式进行容器内部数据的修改。虽然如此，在效率方面也无需担心，`std::initializer_list`的效率是非常高的，它的内部并不负责保存初始化列表中元素的拷贝，仅仅存储了初始化列表中元素的引用。



## using 的使用

### 定义别名

```cpp
// 使用using 代替typedef
typedef unsigned int  uint;
using uint = unsigned int;

// 使用typedef定义函数指针
typedef int(*func_ptr)(int, double);
// 使用using定义函数指针
using func_ptr1 = int(*)(int, double);

```

如果不是特别熟悉函数指针与 typedef，第一眼很难看出 func_ptr 其实是一个别名，其本质是一个函数指针，指向的函数返回类型是 int，函数参数有两个分别是 int，double 类型。

使用 using 定义函数指针别名的写法看起来就非常直观了，把别名的名字强制分离到了左边，而把别名对应的实际类型放在了右边，比较清晰，可读性比较好。

> using 语法和 typedef 一样，并不会创建出新的类型，它们只是给某些类型定义了新的别名。using 相较于 typedef 的优势在于定义函数指针别名时看起来更加直观，并且可以给模板定义别名。
>

### 模板别名

```cpp
// 给模板定义别名
template <typename T>
using mymap = map<int, T>;

int main(void) {
  // map的value指定为string类型
  mymap<string> m;
  m.insert(std::make_pair(1, "luffy"));
  m.insert(std::make_pair(2, "ace"));

  // map的value指定为int类型
  mymap<int> m1;
  m1.insert(std::make_pair(1, 100));
  m1.insert(std::make_pair(2, 200));

  return 0;
}
```



## 可调用对象包装器、绑定器

### 可调用对象

在 C++ 中存在 “可调用对象” 这么一个概念。准确来说，可调用对象有如下几种定义：

1、是一个函数指针

```cpp
int show(int a, int b) {
  printf("a: %d, b: %d", a, b);
  return 0;
}

// func_ptr就是一个函数指针
int (*func_ptr)(int, int) = &show;
// 函数名就是一个指针，指针当中存的是地址，所以不带&也行
int (*func_ptr)(int, int) = show;
```

2、是一个具有`operator()`成员函数的类对象（仿函数）

```cpp
class Test {
 public:
  // 重载小括号，使得类对象可以像函数调用
  void operator()(string msg) { cout << "msg: " << endl; }
};

int main(int argc, char const *argv[]) {
  Test t;
  t("msg: Hello, world");  // 仿函数
  return 0;
}
```

3、是一个可被转换为函数指针的类对象

```cpp
using func_ptr = void (*)(int, string);

class Test {
 public:
  static void print(int a, string b) {
    cout << "name: " << b << ", age: " << a << endl;
  }

  // 将类对象转换为函数指针
  operator func_ptr() { return print; }
};

int main(int argc, char const *argv[]) {
  Test t;
  // 对象转换为函数指针，并调用
  t(19, "Lucky");

  return 0;
}
```

4、是一个类成员函数指针或者类成员指针

```cpp
class Test {
 public:
  int age;

 public:
  void print(int a, string b) {
    cout << "name: " << b << ", age: " << a << endl;
  }
};

int main(int argc, char const *argv[]) {
  // 定义类成员函数指针指向类成员函数
  void (Test::*func_ptr)(int, string) = &Test::print;
  int Test::*obj_ptr = &Test::age;

  Test t;
  // 通过类成员函数指针调用类成员函数
  (t.*func_ptr)(19, "Lucky");
  // 通过类成员指针初始化类成员变量
  t.*obj_ptr = 1;

  cout << "age: " << t.age << endl;

  return 0;
}
```

在上面的例子中满足条件的这些可调用对象对应的类型被统称为可调用类型。C++ 中的可调用类型虽然具有比较统一的操作形式，但定义方式五花八门，这样在我们试图使用统一的方式保存，或者传递一个可调用对象时会十分繁琐。**现在，C++11通过提供`std::function` 和 `std::bind`统一了可调用对象的各种操作。**



### 可调用对象包装器

`std::function`是可调用对象的包装器。它是一个类模板，**可以容纳除了类成员（函数）指针之外的所有可调用对象。**通过指定它的模板参数，它可以用统一的方式处理函数、函数对象、函数指针，并允许保存和延迟执行它们。

`std::function `必须要包含一个叫做 `functional `的头文件，可调用对象包装器使用语法如下:

```cpp
#include <functional>
std::function<返回值类型(参数类型列表)> diy_name = 可调用对象;
```

```cpp
// 普通函数
int add(int a, int b) {
  printf("%d+%d=%d\n", a, b, a + b);
  return a + b;
}

class Test01 {
 public:
    // 静态类成员函数
  static int sub(int a, int b) {
    printf("%d-%d=%d\n", a, b, a - b);
    return a - b;
  }
};

class Test02 {
 public:
  int operator()(int a, int b) {
    printf("%d*%d=%d", a, b, a * b);
    return a * b;
  }
};

int main(int argc, char const *argv[]) {
  // 绑定一个普通函数
  std::function<int(int, int)> f1 = add;
  // 绑定静态类成员函数
  std::function<int(int, int)> f2 = Test01::sub;
  // 绑定一个仿函数
  Test02 t;
  std::function<int(int, int)> f3 = t;

  // 函数调用
  f1(9, 3);
  f2(9, 3);
  f3(9, 3);

  return 0;
}
```

`std::function` 可以将可调用对象进行包装，**得到一个统一的格式，**包装完成得到的对象相当于一个函数指针，和函数指针的使用方式相同，通过包装器对象就可以完成对包装的函数的调用了。

```cpp
class Test01 {
 private:
  std::function<void()> callback;

 public:
  // 通过包装器对象f初始化 callback
  Test01(const std::function<void()>& f) : callback(f) {}
  void notify() {
    // 调用通过构造函数得到的函数指针
    callback();
  }
};

class Test02 {
 public:
  void operator()() { cout << "I want to be a man who is..." << endl; }
};

int main(int argc, char const* argv[]) {
  Test02 t2;
  Test01 t1(t2);  // 仿函数通过包装器对象进行包装

  t1.notify();

  return 0;
}
```

因为回调函数本身就是通过函数指针实现的，使用对象包装器可以取代函数指针的作用。

使用对象包装器 `std::function `可以非常方便的**将仿函数转换为一个函数指针**，通过进行函数指针的传递，在其他函数的合适的位置就可以调用这个包装好的仿函数了。

另外，使用 std::function 作为函数的传入参数，**可以将定义方式不相同的可调用对象进行统一的传递**，这样大大增加了程序的灵活性。



### 绑定器

**`std::bind`用来将可调用对象与其参数一起进行绑定。绑定后的结果可以使用std::function进行保存，并延迟调用到任何我们需要的时候。**通俗来讲，它主要有两大作用：

1. 将可调用对象与其参数一起绑定成一个仿函数。

2. 将多元（参数个数为n，n>1）可调用对象转换为一元或者（n-1）元可调用对象，即只绑定部分参数。

```cpp
// 绑定非类成员函数/变量
auto f = std::bind(可调用对象地址, 绑定的参数/占位符);
// 绑定类成员函/变量
auto f = std::bind(类函数/成员地址, 类实例对象地址, 绑定的参数/占位符);
```

**`std::bind`绑定器返回的是一个仿函数类型，得到的返回值可以直接赋值给一个std::function**，在使用的时候我们并不需要关心绑定器的返回值类型，使用auto进行自动类型推导就可以了。

`placeholders::_1 `是一个占位符，**代表这个位置将在函数调用时被传入的第一个参数所替代。**同样还有其他的占位符 `placeholders::_2`、`placeholders::_3` 等……

```cpp
void output(int x, int y) { cout << x << " " << y << endl; }

int main(int argc, char const *argv[]) {
  auto f1 = std::bind(output, 1, 2);
  f1();

  // 使用绑定器绑定可调用对象和参数，并调用得到的仿函数
  // placeholders指的是参数列表的位置
  std::bind(output, std::placeholders::_1, 2)(10);
  std::bind(output, 2, std::placeholders::_1)(10);

  // error, 调用时没有第二个参数
  //   std::bind(output, 2, std::placeholders::_2)(10);

  // 调用时第一个参数10被吞掉了，没有使用
  std::bind(output, 2, std::placeholders::_2)(10, 20);

  // 交换传入参数位置
  std::bind(output, std::placeholders::_2, std::placeholders::_1)(10, 20);

  return 0;
}
```

std::bind 可以直接绑定函数的所有参数，也可以仅绑定部分参数。在绑定部分参数的时候，**通过使用 std::placeholders 来决定空位参数将会属于调用发生时的第几个参数。**

可调用对象包装器 std::function 是不能实现对类成员函数指针或者类成员指针的包装的，但是通过绑定器 std::bind 的配合之后，就可以完美的解决这个问题了。

```cpp
class Test {
 public:
  int member = 100;

 public:
  void output(int x, int y) { cout << "x: " << x << "y: " << y << endl; }
};

int main(int argc, char const *argv[]) {
  Test t;
  // 绑定类成员函数
  std::function<void(int, int)> f1 = std::bind(
      &Test::output, &t, std::placeholders::_1, std::placeholders::_2);

  // 绑定类成员变量（公共）
  std::function<int &(void)> f2 = std::bind(&Test::member, &t);

  // 调用
  f1(98, 21);
  f2() = 100;
  cout << "t.member: " << t.member << endl;

  return 0;
}
```

在用绑定器绑定类成员函数或者成员变量的时候需要将它们所属的实例对象一并传递到绑定器函数内部。f1的类型是`function<void(int, int)>`，**通过使用std::bind将Test的成员函数output的地址和对象t绑定，并转化为一个仿函数并存储到对象f1中。**

使用绑定器绑定的类成员变量`member`得到的仿函数被存储到了类型为function<int&(void)>的包装器对象f2中，并且可以在需要的时候修改这个成员。其中int是绑定的类成员的类型，并且允许修改绑定的变量，因此需要指定为变量的引用，由于没有参数因此参数列表指定为void。

示例程序中是使用 function 包装器保存了 bind 返回的仿函数，**如果不知道包装器的模板类型如何指定，可以直接使用 auto 进行类型的自动推导，这样使用起来会更容易一些。**



## POD类型

POD 是英文中 `Plain Old Data `的缩写，翻译过来就是 **普通的旧数据** 。POD 在 C++ 中是非常重要的一个概念，通常用于说明一个类型的属性，尤其是用户自定义类型的属性。

POD 属性在 C++11 中往往又是构建其他 C++ 概念的基础，事实上，在 C++11 标准中，POD 出现的概率相当高。因此学习 C++，尤其是在 C++11 中，了解 POD 的概念是非常必要的。

Plain ：表示是个普通的类型

Old ：体现了其与 C 的兼容性，支持标准 C 函数
在 C++11 中将 POD 划分为两个基本概念的合集，即∶**平凡的（trivial） 和标准布局的（standard layout ）。**



### "平凡"类型

一个平凡的类或者结构体应该符合以下几点要求：

1、拥有平凡的默认构造函数（trivial constructor）和析构函数（trivial destructor）。

- 平凡的默认构造函数就是说构造函数什么都不干。通常情况下，不定义类的构造函数，编译器就会为我们生成一个平凡的默认构造函数。**一旦定义了构造函数**，即使构造函数不包含参数，函数体里也没有任何的代码，**那么该构造函数也不再是"平凡"的。**

- 关于析构函数也和上面列举的构造函数类似，一旦被定义就不平凡了。但是这也并非无药可救，**使用 =default 关键字可以显式地声明默认的构造函数，从而使得类型恢复 “平凡化”。**


2、拥有平凡的拷贝构造函数（trivial copy constructor）和移动构造函数（trivial move constructor）。

- 平凡的拷贝构造函数基本上等同于使用 memcpy 进行类型的构造。
- 同平凡的默认构造函数一样，不声明拷贝构造函数的话，编译器会帮程序员自动地生成。
- 可以显式地使用 = default 声明默认拷贝构造函数。
- 而平凡移动构造函数跟平凡的拷贝构造函数类似，只不过是用于移动语义。

3、拥有平凡的拷贝赋值运算符（trivial assignment operator）和移动赋值运算符（trivial move operator）。

* 这基本上与平凡的拷贝构造函数和平凡的移动构造运算符类似。

4、不包含虚函数以及虚基类。

- 类中使用 virtual 关键字修饰的函数 叫做虚函数。
- 虚基类是在创建子类的时候在继承的基类前加 virtual 关键字 修饰。




### "标准布局"类型

标准布局类型主要主要指的是类或者结构体的结构或者组合方式。标准布局类型的类应该符合以下五点定义，最重要的为前两条：

1、所有非静态成员有相同 的访问权限（public，private，protected）。

2、在类或者结构体继承时，满足以下两种情况之一∶

- 派生类中有非静态成员，基类中包含静态成员（或基类没有变量）。
- 基类有非静态成员，而派生类没有非静态成员。

> **非静态成员只要同时出现在派生类和基类间，即不属于标准布局。**
>
> 对于多重继承，一旦非静态成员出现在多个基类中，即使派生类中没有非静态成员变量，派生类也不属于标准布局。

3、子类中第一个非静态成员的类型与其基类不同。

这条规则对于我们来说是比较特别的，这样规定的目的主要是是节约内存，提高数据的读取效率。对于上面的两个子类 Child 和 Child1 来说它们的内存结构是不一样的，在基类没有成员的情况下：

- C++ 标准允许标准布局类型派生类的第一个成员与基类共享地址，此时基类并没有占据任何的实际空间（可以节省一点数据）

- 对于子类 Child 而言，如果子类的第一个成员仍然是基类类型，C++ 标准要求类型相同的对象它们的地址必须不同，此时需要分配额外的地址空间将二者的地址错开。


```CPP
class Parent{};
class Child1 : public Parent {
    int foo;   // 子类的第一个非静态成员，和父类的类型不同
    
    Parent p;	// 和父类类型相同
};
```

4、没有虚函数和虚基类。

5、所有非静态数据成员均符合标准布局类型，其基类也符合标准布局，这是一个递归的定义。



### 对"平凡"类型判断

C++11 提供的类模板叫做 is_trivial，其定义如下：

```cpp
template <class T> struct std::is_trivial;
```

`std::is_trivial `的成员 value 可以用于判断 T 的类型是否是一个平凡的类型（value 函数返回值为布尔类型）。除了类和结构体外，is_trivial 还可以对内置的标准类型数据（比如 int、float 都属于平凡类型）及数组类型（元素是平凡类型的数组总是平凡的）进行判断。

```cpp
#include <type_traits>
class A {};
class B {
  B() {}
};
class C : public B {};
class D {
  virtual void func() {}
};

class E : virtual public A {};

int main(int argc, char const *argv[]) {
  cout << std::boolalpha;  // 让输出流将bool类型转换为true或者false
  // 内置标准数据类型，属于 trivial 类型
  cout << "int: " << std::is_trivial<int>::value << endl;
  // 拥有默认的构造和析构函数，属于 trivial 类型
  cout << "A: " << std::is_trivial<A>::value << endl;
  // 自定义了构造函数
  cout << "B: " << std::is_trivial<B>::value << endl;
  // 基类中自定义了构造函数，因此不属于 trivial 类型
  cout << "C: " << std::is_trivial<C>::value << endl;
  // 类成员函数中有虚函数，因此不属于 trivial 类型
  cout << "D: " << std::is_trivial<D>::value << endl;
  // 继承关系中有虚基类，因此不属于 trivial 类型
  cout << "E: " << std::is_trivial<E>::value << endl;

  return 0;
}
```



### 对"标准布局"类型的判断

```cpp
template <typename T> struct std::is_standard_layout;
```

通过` is_standard_layout` 模板类的成员 value（is_standard_layout<T>∶∶value），我们可以在代码中打印出类型的标准布局属性，函数返回值为布尔类型。

```cpp
#include <type_traits>
struct A {};
struct B : A {
  int j;
};
struct C {
 public:
  int a;

 private:
  int c;
};
struct D1 {
  static int i;
};
struct D2 {
  int i;
};
struct E1 {
  static int i;
};
struct E2 {
  int i;
};
struct D : public D1, public E1 {
  int a;
};
struct E : public D1, public E2 {
  int a;
};
struct F : public D2, public E2 {
  static int a;
};
struct G : public A {
  int foo;
  A a;
};
struct H : public A {
  A a;
  int foo;
};

int main() {
  cout << std::boolalpha;
  cout << "is_standard_layout:" << std::endl;
  // 没有虚基类和虚函数，属于 standard_layout 类型
  cout << "A: " << is_standard_layout<A>::value << endl;
  // 没有虚基类和虚函数，属于 standard_layout 类型
  cout << "B: " << is_standard_layout<B>::value << endl;
  // 所有非静态成员访问权限不一致，不属于 standard_layout 类型
  cout << "C: " << is_standard_layout<C>::value << endl;
  // 基类和子类没有同时出现非静态成员变量，属于 standard_layout 类型
  cout << "D: " << is_standard_layout<D>::value << endl;
  // 没有虚基类和虚函数，属于 standard_layout 类型
  cout << "D1: " << is_standard_layout<D1>::value << endl;
  // 基类和子类中同时出现了非静态成员变量，不属于 standard_layout 类型
  cout << "E: " << is_standard_layout<E>::value << endl;
  // 多重继承中在基类里同时出现了非静态成员变量，不属于 standard_layout 类型
  cout << "F: " << is_standard_layout<F>::value << endl;
  // 使用的编译器不同，得到的结果也不同。
  cout << "G: " << is_standard_layout<G>::value << endl;
  // 子类中第一个非静态成员的类型与其基类类型不能相同，不属于 standard_layout
  // 类型
  cout << "H: " << is_standard_layout<H>::value << endl;
  return 0;
}
```

### 总结

事实上，我们使用的很多内置类型默认都是 POD 的。POD 最为复杂的地方还是在类或者结构体的判断。不过上面也给大家介绍了判断的方法，相信大家对 POD 已经有所理解。那么，使用 POD 有什么好处呢？

1. **字节赋值**，代码中我们可以安全地使用 memset 和 memcpy 对 POD 类型进行初始化和拷贝等操作。
2. **提供对 C 内存布局兼容**。C++ 程序可以与 C 函数进行相互操作，因为 POD 类型的数据在 C 与 C++ 间的操作总是安全的。
3. **保证了静态初始化的安全有效**。静态初始化在很多时候能够提高程序的性能，而 POD 类型的对象初始化往往更加简单。

关于 POD 重在理解，我们在查阅资料的时候经常会看到引用 POD 的地方，所以建议大家花时间消化一下这个概念。



## 默认函数控制 =default 和 =delete

### 类与默认函数

在 C++ 中声明自定义的类，编译器会默认帮助程序员生成一些他们未自定义的成员函数。这样的函数版本被称为” 默认函数”。这样的函数一共有六个，我们一起来看一下：

`无参构造函数`：创建类对象。`拷贝构造函数`：拷贝类对象。`移动构造函数`：拷贝类对象

`拷贝赋值函数`：类对象赋值。`移动赋值函数`：类对象赋值。`析构函数`：销毁类对象

在 C++ 语法规则中，一旦程序员实现了这些函数的自定义版本，则编译器不会再为该类自动生成默认版本。有时程序员会忘记上面提到的规则，最常见的是声明了带参数的构造，如果还需要无参构造函数，这时候必须定义出不带参数的版本。不过通过编译器的提示，这样的问题通常会得到更正。

但更为严重的问题是，一旦声明了自定义版本的构造函数，则有可能导致我们定义的类型不再是 `POD 类型`，我们便不再能够享受 POD 类型为我们带来的便利。C++11 非常贴心地为我们提供了解决方案，就是使用 `=default `

### =default

在 C++11 标准中称 = default 修饰的函数为**显式默认【缺省】（explicit defaulted）函数**，而称 =delete 修饰的函数为**删除（deleted）函数或者显示删除函数**。

C++11 引入显式默认和显式删除是为了增强对类默认函数的控制，让程序员能够更加精细地控制默认版本的函数。

我们可以在类内部修饰满足条件的类函数为显示默认函数，也可以在类定义之外修饰成员函数为默认函数。**使用 `=defaut` 指定的默认函数和类提供的默认函数是等价的**

```cpp
// 类定义
class Base {
 public:
  Base() = default;  // 在类内指定成员函数为默认函数
  Base(const Base& obj) = default;
  Base(Base&& obj);
  Base& operator=(const Base& obj);
  Base& operator=(Base&& obj);
  ~Base();

  // error, 自定义带参构造，不允许使用 =default 修饰
  Base(int a, int b) = default;
  // error, 自定义函数，不允许使用 =default 修饰
  void print() = default;
  // error, 不是移动、复制赋值运算符重载，不允许使用 =default 修饰
  bool operator==(const Base& obj) = default;
};

// 在类定义之外指定成员函数为默认函数
Base::Base(Base&& obj) = default;
Base& Base::operator=(const Base& obj) = default;
Base& Base::operator=(Base&& obj) = default;
Base::~Base() = default;
```

> 如果程序猿对 C++ 类提供的默认函数（上面提到的六个函数）进行了实现，**那么可以通过 =default 将他们再次指定为默认函数，不能使用 =default 修饰这六个函数以外的函数。**



### =delete

=delete 表示显示删除，`显式删除可以避免用户使用一些不应该使用的类的成员函数`，使用这种方式可以有效的防止某些类型之间自动进行隐式类型转换产生的错误。

```cpp
class Base {
 public:
  Base() = default;

  // 禁止使用拷贝构造函数
  Base(const Base& obj) = delete; 
  // 禁止使用 = 进行对象复制
  Base& operator=(const Base& obj) = delete;
  // 禁止使用带char类型参数，防止隐式类型转换（char转int）
  Base(char c) = delete;
  // 禁使用带char类型参数的print函数
  void print(char c) = delete;
};
```



## 扩展的friend语法

friend 关键字在 C++ 中是一个比较特别的存在。因为在大多数编程语言中是没有提供 friend 关键字的，比如 Java。friend 关键字用于声明类的友元，友元可以无视类中成员的属性（ public、protected 或是 private ），友元类或友元函数都可以访问，这就完全破坏了面向对象编程中封装性的概念。但有的时候，friend 关键字确实会让程序猿少写很多代码，因此 friend 还是在很多程序中被使用到。

### 语法改进

**声明一个类为另外一个类的友元时，不再需要使用 class 关键字，**并且还可以使用类的别名（使用 typedef 或者 using 定义）。

```cpp
// 类声明
class Lucky;

class Rocky {
  //   friend class Lucky;  // c++98标准
  friend Lucky;  // c++11标准

 private:
  string name = "Rocky";
};

class Nancy {
  friend Lucky;

 protected:
  string name = "Nancy";
};

class Lucky {
 private:
  Rocky rocky;
  Nancy nancy;

 public:
  void show() {
    // 可调用友元的私有或者保护成员
    cout << "private member: " << rocky.name << endl;
    cout << "protect member: " << nancy.name << endl;
  }
};

int main(int argc, char const *argv[]) {
  Lucky lucky;
  lucky.show();

  return 0;
}
```



### 为类模板声明友元

```cpp
class Tom;

template <typename T>
class Person {
  friend T;
};

int main() {
  // Tom类是 Person类的友元
  Person<Tom> p1;
  // 对于 int 类型的模板参数，友元声明被忽略（第 6 行）
  Person<int> p2;
  return 0;
}
```

这样一来，我们在模板实例化时才确定一个模板类是否有友元，以及谁是这个模板类的友元。

## 强制类型枚举

### 枚举

```cpp
// 匿名枚举
enum {Red, Green, Blue};
// 有名枚举
enum Colors{Red, Green, Blue};
```

枚举类型中的枚举值编译器会默认从 0 开始赋值，而后依次向下递增，也就是说 Red=0，Green=1，Blue=2。

C/C++ 的 enum 有个很” 奇怪” 的设定，**就是具名（有名字）的enum类型的名字，以及 enum 的成员的名字都是全局可见的。**这与 C++ 中具名的 namespace、class/struct 及 union 必须通过名字::成员名的方式访问相比是格格不入的，编码过程中一不小心程序员就容易遇到问题。

另外，由于 C 中枚举被设计为常量数值的” 别名” 的本性，所以枚举的成员总是可以被隐式地转换为整型，但是很多时候我们并不想这样。

```cpp
// 两个具名的枚举是全局可见的，所以TOMATO命名冲突
enum Fruit { APPLE, TOMATO };
enum Vegetable { POTATO, TOMATO };
```



### 强制类型枚举

针对枚举的缺陷，C++11 标准引入了一种新的枚举类型，即枚举类，又称强类型枚举（strong-typed enum）。声明强类型枚举非常简单，只需要在 enum 后加上关键字 class。

```cpp
// 定义强类型枚举
enum class Colors{Red, Green, Blue};
```

强类型枚举具有以下几点优势∶

强作用域，强类型枚举成员的名称不会被输出到其父作用域空间。

强类型枚举只能是有名枚举，如果是匿名枚举会导致枚举值无法使用（因为没有作用域名称）。
转换限制，强类型枚举成员的值不可以与整型隐式地相互转换。

可以指定底层类型。强类型枚举默认的底层类型为 int，但也可以显式地指定底层类型，具体方法为在枚举名称后面加上`∶type`，其中 type 可以是除 `wchar_t `以外的任何整型。

```cpp
enum class Colors :char { Red, Green, Blue };
```

> wchar_t 是什么？
>
> 双字节类型，或宽字符类型，是 C/C++ 的一种扩展的存储方式，一般为 16 位或 32 位，所能表示的字符数远超 char 型。
>
> 主要用在国际化程序的实现中，但它不等同于 `unicode `编码。unicode 编码的字符一般以 `wchar_t `类型存储。



```cpp
enum class China {
  Shanghai,
  Dongjing,
  Beijing,
  Nanjing,
};
enum class Japan : char { Dongjing, Daban, Hengbin, Fudao };

int main() {
  // error, 强类型枚举属于强作用于类型，不能直接使用，枚举值前必须加枚举类型
  int m = Shanghai;
  // error, 强类型枚举不会进行隐式类型转换，因此枚举值不能直接给 int 行变量赋值
  //（虽然强类型枚举的枚举值默认就是整形，但其不能作为整形使用）。
  int n = China::Shanghai;

  int k = (int)China::Shanghai;  // ok
  printf("size: %d\n"), sizeof(China::Dongjing);
  printf("size: %d\n"), sizeof(Japan::Dongjing);
  return 0;
}
```



### 对原有枚举的扩展

C++11还对原有的枚举类型进行了扩展

1、原有枚举类型的底层类型在默认情况下，仍然由编译器来具体指定实现。**但也可以跟强类型枚举类一样，显式地由程序员来指定。**其指定的方式跟强类型枚举一样，都是枚举名称后面加上`∶type`，其中 type 可以是除 `wchar_t` 以外的任何整型。

```cpp
enum Colors : char { Red, Green, Blue };
```

2、关于作用域，在 C++11 中，枚举成员的名字除了会自动输出到父作用域，也可以在枚举类型定义的作用域内有效。

```cpp
enum Colors : char { Red, Green, Blue };
int main() {
  Colors c1 = Green;          // C++11以前的用法
  Colors c2 = Colors::Green;  // C++11的扩展语法
  return 0;
}
```

我们在声明强类型枚举的时候，也可以使用关键字 `enum struct`，效果与`enum struct`完全一样。

## 非受限联合体

联合体又叫共用体，我将其称之为 `union`，它的使用方式和结构体类似，程序猿可以在联合体内部定义多种不同类型的数据成员，**但是这些数据会共享同一块内存空间（也就是如果对多个数据成员同时赋值会发生数据的覆盖）。**在某些特定的场景下，通过这种特殊的数据结构我们就可以实现内存的复用，从而达到节省内存空间的目的。

在 C++11 之前我们使用的联合体是有局限性的，主要有以下三点：

1. 不允许联合体拥有非 POD 类型的成员
2. 不允许联合体拥有静态成员
3. 不允许联合体拥有引用类型的成员

在新的 C++11 标准中，取消了关于联合体对于数据成员类型的限定，**规定任何非引用类型都可以成为联合体的数据成员，这样的联合体称之为非受限联合体（`Unrestricted Union`）**

### 静态类型成员

对于非受限联合体来说，静态成员有两种分别是静态成员变量和静态成员函数。

```cpp
union Test {
  int age;
  long id;
  // int& tmp = age;    // error,非受限联合体不允许出现引用类型
  static char ch;
  static int print() {
    printf("ch value: %c\n", ch);
    return 0;
  }
};

char Test::ch = 'a';

int main(int argc, char const *argv[]) {
  Test t1;
  Test t2;
  t1.ch = 'b';
  t2.ch = 'c';
  t2.age = 19;

  cout << "t1.ch: " << t1.ch << endl;
  cout << "t2.ch: " << t2.ch << endl;
  cout << "t2.age: " << t2.age << endl;
  cout << "t2.id: " << t2.id << endl;

  t1.print();
  Test::print();

  return 0;
}
```

- 非受限联合体中的静态成员变量需要在非受限联合体外部声明或者初始化之后才能使用。
- 通过打印的结果可以发现 `t1和t2 `对象共享这个静态成员变量（和类 class/struct 中的静态成员变量的使用是一样的）。
- 非受限联合体中的静态成员函数在静态函数 print() 只能访问非受限联合体 Test 中的静态变量，对于非静态成员变量（age、id）是无法访问的。
- 调用这个静态方法可以通过对象也可以通过类名实现。
- **在非受限联合体中静态成员变量和非静态成员变量使用的不是同一块内存。**



### 非POD类型成员

在 C++11 标准中会默认删除一些非受限联合体的默认函数。比如，非受限联合体有一个非 POD 的成员，而该非 POD 成员类型拥有 非平凡的构造函数，那么非受限联合体的默认构造函数将被编译器删除。其他的特殊成员函数，例如默认拷贝构造函数、拷贝赋值操作符以及析构函数等，也将遵从此规则。



```cpp
union Student {
  int id;
  string name;
};

int main() {
  Student s;
  return 0;
}
```

上面代码中的非受限联合体 Student 中拥有一个非 PDO 类型的成员 string name，**string 类中有非平凡构造函数**，因此 Student 的构造函数被删除（通过警告信息可以得知它的析构函数也被删除了）导致对象无法被成功创建出来。解决这个问题的办法就是由程序猿自己为非受限联合体定义构造函数，在定义构造函数的时候我们需要用到定位**放置 new（placement new）** 操作。



### placement new

一般情况下，使用 new 申请空间时，是从系统的堆（heap）中分配空间，申请所得的空间的位置是根据当时的内存的实际使用情况决定的。但是，在某些特殊情况下，可能需要在已分配的特定内存创建对象，这种操作就叫做 `placement new `即**定位放置 new。**

```cpp
// 使用 new 申请内存空间：
Base* ptr = new Base;

// 使用定位放置 new 申请内存空间：
ClassName* ptr = new (定位的内存地址)ClassName;
```

使用定位放置的方式为指针 b 申请了一块内存，也就是说此时指针 b 指向的内存地址和变量 n 对应的内存地址是同一块（栈内存），而在 Base类中成员变量 number 的起始地址和 Base对象的起始地址是相同的，所以打印出 number 的值为 100 也就是整形变量 n 的值。

```cpp
class Base {
 private:
  int number;

 public:
  Base() {}
  ~Base() {}
  void print() { cout << "number value: " << number << endl; }
};

int main() {
  int n = 100;
  Base* b = new (&n) Base;
  b->print();
  return 0;
}
```



最后，给大家总结一下关于 placement new 的一些细节：

- 使用定位放置 new 操作，既可以在栈 (stack) 上生成对象，也可以在堆（heap）上生成对象，这取决于定位时指定的内存地址是在堆还是在栈上。
- 从表面上看，定位放置 new 操作是申请空间，**其本质是利用已经申请好的空间，真正的申请空间的工作是在此之前完成的。**
- 使用定位放置 new 创建对象时会自动调用对应类的构造函数，但是由于对象的空间不会自动释放，如果需要释放堆内存必须显示调用类的析构函数（里面需要使用delete或者free语句）。
- **使用定位放置 new 操作，我们可以反复动态申请到同一块堆内存，这样可以避免内存的重复创建销毁，从而提高程序的执行效率（比如网络通信中数据的接收和发送）。**



### 自定义非受限联合体构造函数

```cpp
class Base {
 private:
  string msg;

 public:
  void setMSG(string str) { msg = str; }
  void print() { cout << "Base msg: " << msg << endl; }
};

union Student {
  int id;
  Base tmp;
  string name;

  Student() { new (&name) string; }
  ~Student() {}
};

int main() {
  Student s;
  s.name = "蒙奇·D·路飞";
  s.tmp.setMSG("我是要成为海贼王的男人!");
  s.tmp.print();
  cout << "Student name: " << s.name << endl;
  return 0;
}

```

我们在上面的程序里边给非受限制联合体显示的指定了构造函数和析构函数。

创建一个非受限联合体对象，这时便调用了联合体内部的构造函数，在构造函数里通过定位放置 new 的方式将构造出的对象地址定位到了联合体的成员 string name 的地址上了，这样联合体内部其他非静态成员也就可以访问这块地址了（通过输出的结果可以看到对联合体内的 tmp 对象赋值，会覆盖 name 对象中的数据）。



### 匿名的非受限联合体

一般情况下我们使用的非受限联合体都是具名的（有名字），但是我们也可以定义匿名的非受限联合体，一个比较实用的场景就是配合着类的定义使用。我们来设定一个场景：

> 木叶村要进行第99次人口普查，人员的登记方式如下：
>
>    - 学生只需要登记所在学校的编号
>    - 本村学生以外的人员需要登记其身份证号码
>    - 本村外来人员需要登记户口所在地+联系方式

```cpp
// 外来人口信息
struct Foreigner {
  Foreigner(string s, string ph) : addr(s), phone(ph) {}
  string addr;
  string phone;
};

// 登记人口信息
class Person {
 public:
  enum class Category : char { Student, Local, Foreign };
  Person(int num) : number(num), type(Category::Student) {}
  Person(string id) : idNum(id), type(Category::Local) {}
  Person(string addr, string phone)
      : foreign(addr, phone), type(Category::Foreign) {}
  ~Person() {}

  void print() {
    cout << "Person category: " << (int)type << endl;
    switch (type) {
      case Category::Student:
        cout << "Student school number: " << number << endl;
        break;
      case Category::Local:
        cout << "Local people ID number: " << idNum << endl;
        break;
      case Category::Foreign:
        cout << "Foreigner address: " << foreign.addr
             << ", phone: " << foreign.phone << endl;
        break;
      default:
        break;
    }
  }

 private:
  Category type;
  union {
    int number;
    string idNum;
    Foreigner foreign;
  };
};

int main() {
  Person p1(9527);
  Person p2("1101122022X");
  Person p3("砂隐村村北", "1301810001");
  p1.print();
  p2.print();
  p3.print();
  return 0;
}

```

根据需求我们将木叶村的人口分为了三类并通过枚举记录了下来，在 Person类中添加了一个匿名的非受限联合体用来存储人口信息，仔细分析之后就会发现这种处理方式的优势非常明显：尽可能地节省了内存空间。

Person类可以直接访问匿名非受限联合体内部的数据成员。

不使用匿名非受限联合体申请的内存空间等于 number、 idNum 、 foreign 三者内存之和。

使用匿名非受限联合体之后 number、 idNum 、 foreign 三者共用同一块内存。















## Reference

本文大部分摘自苏丙榅 https://subingwen.com/



















































































