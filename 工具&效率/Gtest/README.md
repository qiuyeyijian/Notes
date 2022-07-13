# Gtest

## Gtest简介与安装：

Google Test(Gtest)是一个跨平台(Liunx、Mac OS X、Windows、Cygwin、Windows CE and Symbian)C++单元测试框架，由google公司发布。gtest是为在不同平台上为编写C++测试而生成的。它提供了丰富的断言、致命和非致命判断、参数化、”死亡测试”等等。

安装：

Windows：在需要安装Gtest框架的项目中，打开项目->工具->NuGet程序包管理器->程序包管理控制台，随后在下方会出

随后打开浏览器，搜索并进入Nuget gallery的官网，然后在搜索栏中搜索：googletest，双击进入，复制program manager下的url，黏贴到程序包管理控制台中PM>的后面，然后回车。

等待安装，当显示如下文本则表示安装成功：

 

 

## 断言

断言是一组用于测试函数和类的功能的宏，断言分为两种：致命断言(ASSERT系列)和非致命断言(EXPECT系列)。

ASSERT系列断言在报错后，其所在的测试用例会终止并且返回，而EXPECT系列断言在报错后，测试用例会继续运行，不会影响其他的测试用例。由于这种特性，ASSERT系列断言通常用于比较函数返回值来确认函数是否正常运行和，这种情况下，如果函数本身都无法正常运行，那么后面的其余测试可能就没有测试的必要了。而EXPECT系列断言则用于在测试用例中收集更多的错误，提升测试的效率。

### 二元比较：

这一类断言是最常用的，可以用来比较整数与std::string，本质上是比较指针。

 

### 条件判断

这一类断言用来判断给定的表达式是否为真或是否为假：】

 

### 谓词断言

这一类断言可以让我们自建谓词逻辑进行断言：

 

### 死亡测试：

这一类断言是用来创建一些可能导致程序崩溃的代码，使用ASSERT_DEATH/EXPECT_DEATH可以安全的测试程序是否会按预定的情况崩溃。

### 字符串比较：

这一类断言用于比较C风格字符串，如果要比较std::string的话需要使用二元比较断言：



 

 

## Gtest事件机制

首先说明gtest中事件的结构层次：

 

### 全局事件：

实现全局的事件机制，需要创建一个自己的类，然后继承testing::Environment类，然后分别实现成员函数SetUp()和TearDown()，

同时在main函数内进行调用，即"testing::AddGlobalTestEnvironment(new MyEnvironment);"，通过调用函数我们可以添加多个全局的事件机制。
 

SetUp()函数是在所有测试开始前执行。

TearDown()函数是在所有测试结束后执行。



### Testcase事件

测试用例的事件机制的创建和测试套件的基本一样，不同地方在于测试用例实现的两个函数分别是SetUp()和TearDown(),这两个函数不是静态函数了。

SetUp()函数是在一个测试用例的开始前执行。

TearDown()函数是在一个测试用例的结束后执行。

 

### Testsuite事件

测试套件的事件机制我们同样需要去创建一个类，继承testing::Test，实现两个静态函数SetUpTestCase()和TearDownTestCase()，测试套件的事件机制不需要像全局事件机制一样在main注册，而是需要将我们平时使用的TEST宏改为TEST_F宏。

SetUpTestCase()函数是在测试套件第一个测试用例开始前执行。

TearDownTestCase()函数是在测试套件最后一个测试用例结束后执行。



### TEST_P宏

TEST宏用于创建基于环境的测试案例和非类依赖的测试案例

TEST_F宏用于需要相同的变量初始化的时候，创建类依赖的测试案例。

而TEST_P宏则用于需要使用不同参数进行测试的时候。

 

\1. 要写值参数化测试，首先应该定义一个fixture类。 它必须继承:: testing :: Test和:: testing :: WithParamInterface <T>（后者是纯粹的接口），其中T是参数值的类型。

可以从:: testing :: TestWithParam <T>派生fixture类，它本身是从:: testing :: Test和:: testing :: WithParamInterface <T>派生的。 T可以是任何可复制类型。 如果它是一个原始指针，你负责管理指向的值的生命周期。

2.告诉gtest拿到参数的值后，具体做些什么样的测试

在TEST_P宏里，使用GetParam()获取当前的参数的具体值。

可以使用INSTANTIATE_TEST_CASE_P来实例化具有任何您想要的参数的测试用例。 Google Test定义了一些用于生成测试参数的函数。 它们返回我们所谓的参数生成器（surprise！）。 这里是它们的摘要，它们都在testing命名空间中：

 

INSTANTIATE_TEST_CASE_P的第一个参数是测试案例的前缀，可以任意取。 

第二个参数是测试案例的名称，需要和之前定义的参数化的类的名称相同，如：IsPrimeParamTest 

第三个参数是可以理解为参数生成器，上面的例子使用test::Values表示使用括号内的参数。Google提供了一系列的参数生成的函数：

 

 