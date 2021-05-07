# 立即执行函数 (function() {})()



通常我们声明一个函数有以下几种方式：

```javascript
// 声明函数f1
function f1() {
    console.log("f1");
}
// 通过()来调用此函数
f1();


//一个匿名函数的函数表达式，被赋值给变量f2:
var f2 = function() {
    console.log("f2");
}
//通过()来调用此函数
f2();


//一个命名为f3的函数的函数表达式(这里的函数名可以随意命名，可以不必和变量f3重名)，被赋值给变量f3:
var f3 = function f3() {
    console.log("f3");
}
//通过()来调用此函数
f3();
```

如果你看过一些自定义控件的话你会发现他们大多数都是沿用这种写法：

~~~javascript
(function() {
    ```
   // 这里开始写功能需求
 })();  
~~~

这是我们常说的立即执行函数(IIFE)，顾名思义，也就是说这个函数是立即执行函数体的，不需要你额外去主动的去调用，一般情况下我们只对匿名函数使用IIFE，这么做有两个目的：

> 一是不必为函数命名，避免了污染全局变量 二是IIFE内部形成了一个单独的作用域，可以封装一些外部无法读取的私有变量。

如果看到这两句话无法理解，那么先从IIFE的运行原理说起。 因为IIFE通常用于匿名函数，这里就用简单的匿名函数作为栗子：

```javascript
var f = function(){
    console.log("f");
}
f();
```

我们发现这里`f`只是这个匿名函数的一个引用变量，那么既然`f()`能够调用这个函数，我把`f`替换成函数本身可以么：

```javascript
function(){
   console.log("f");    
}();
```

运行之后得到如下结果：

```javascript
Uncaught SyntaxError: Unexpected token (
```

产生这个错误的原因是，Javascript引擎看到function关键字之后，认为后面跟的是函数声明语句，不应该以圆括号结尾。解决方法就是让引擎知道，圆括号前面的部分不是函数定义语句，而是一个表达式，可以对此进行运算，这里区分一下函数声明和函数表达式：

```javascript
1、函数声明(即我们通常使用function x(){}来声明一个函数)
function myFunction () { /* logic here */ }
2、函数表达式(类似以这种的形式)
var myFunction = function () { /* logic here */ };
var myObj = {
    myFunction: function () { /* logic here */ }
};
```

小学我们就学过用`()`括起来的表达式会先执行，就像下面这样：

```
1+(2+3) //这里先运行小括号里面的内容没有意见撒
```

其实在`javascript`中小括号也有相似的作用，Javascript引擎看到function关键字会认为是函数声明语句，那么如果Javascript引擎优先看到小括号会怎么样：

```javascript
//用小括号把函数包裹起来
(function(){
   console.log("f");    
})();
```

函数成功执行了：

```
f //控制台输出
```

这种情况下Javascript引擎就会认为这是一个表达式，而不是函数声明，当然要让Javascript引擎认为这是一个表达式的方法还有很多：

```
!function(){}();
+function(){}();
-function(){}();
~function(){}();
new function(){ /* code */ }
new function(){ /* code */ }() // 只有传递参数时，才需要最后那个圆括号。
……
```

回到前面的问题，为什么说IIFE这种形式避免了污染全局变量，如果你见过别人写的jquery插件，里面通常会有类似这样的代码：

~~~javascript
(function($){
    ```
   //插件实现代码
})(jQuery);
~~~

这里的`jquery`其实是该匿名函数的参数，联想一下我们调用匿名函数时候是用`f()`那么匿名带参数的就是`f(args)`对吧，这里把jquery作为参数传入该函数，那么在函数内部使用形参`$`的时候就不会影响到外部环境，因为有些插件也会用到`$`这个限定符，你在这个函数内部可以随意折腾。

以上，在此过程中参考了以下两篇文章：
[javascript立即执行某个函数：插件中function(){}()再思考](http://www.tangshuang.net/2020.html)
[JavaScript中的立即执行函数](https://segmentfault.com/a/1190000003902899)