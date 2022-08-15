## 第一章 TypeScript介绍

#### 1.简介

- TS是是由微软2012年开发的一款开源的编程语言
- TypeScript 是 Javascript 的超集，遵循最新的 ES6、Es5 规范。TypeScript 扩展了 JavaScript
  的语法
- 在js的基础上，为js添加了**类型支持**

#### 2.设计目标

- 遵循当前以及未来出现的ECMAScript规范
- 开发大型应用，可以编译程纯JavaScript,编译出来的JavaScript可以运行在浏览器上
- 成为跨平台的开发工具，TypeScript使用Apache作为开源协议，且能够再所有主流的操作系统上安装和执行

#### 3.TS优势

- 更早的发现错误
- 任何位置都有**代码提示**，增加开发效率
- 类型系统提升了代码的可维护性，重构更容易
- 使用最新的ECMAScript语法，最新
- ts类型推断机制，降低成本

#### 4.TS劣势

- 短期投入到工作可能增加开发成本
- 和有些库的结合不是很完美
- 学习需要成本，需要理解接口，泛型，类型等知识
- 集成到自动构建流程中需要额外的工作量

#### 5.TS与JS的区别

![1657037907209](C:\Users\11\AppData\Roaming\Typora\typora-user-images\1657037907209.png)

| JavaScript                 | TypeScript                                             |
| -------------------------- | ------------------------------------------------------ |
| 动态语言                   | 具有静态语言的特点                                     |
| 编译性语言运行时报错       | 编译期间报错                                           |
| 弱类型语言，没有类型       | 强类型语言，类似java, C++等，定义时指明类型            |
| 不支持模块、接口、泛型     | 支持模块、接口、泛型                                   |
| 基本数据类型和引用数据类型 | 更多的基本数据类型和引用数据类型，如any, never, enum等 |
| 在浏览器中直接执行         | 编译为js后才能在浏览器进行执行                         |





## 第二章 TS环境安装与初体验

#### 1.安装与运行

```js
npm install -g typescript
或者
cnpm install -g typescript
或者
yarn global add typescript
```

```js
tsc 文件名称.ts

tsc --watch  
```

#### 2.js缺陷的演示

```javascript
/* 
  1.没有对类型进行检测
  2.没有对是否传参进行检测
*/
function test(msg) {
  console.log(msg.length);
}

test("邱淑贞") // 可以正常使用
test(666) // undefine
test() // error
console.log("往后余生,风雪是你, 平淡是你,敲每一行代码想的都是你。");
console.log("你是CSS,我是DIV,就算我的布局再好,没了你也就没了色彩。");
```

采用TS书写同样的代码，我们可以看到，编译器非常友好的对我们进行了提示

```typescript
function test(msg) {
  console.log(msg.length);
}

test("邱淑贞") // 可以正常使用
test(666) // undefine
test() // error
console.log("往后余生,风雪是你, 平淡是你,敲每一行代码想的都是你。");
console.log("你是CSS,我是DIV,就算我的布局再好,没了你也就没了色彩。");
```

![1657039512822](C:\Users\11\AppData\Roaming\Typora\typora-user-images\1657039512822.png)



## 第三章 TS中的数据类型

#### 1.基础数据类型 

- number类型： 双精度 64 位浮点值。它可以用来表示整数和分数。

- boolean类型：表示逻辑值：true 和 false。
- string类型：一个字符系列，使用单引号（**'**）或双引号（**"**）来表示字符串类型。**反引号（`）来定义多行文本和内嵌表达式**

```typescript
export default {}

// 数值类型 number
let money:number; // 定义了一个名称叫做money的变量, 这个变量中将来只能存储数值类型的数据
money = 20;
// money = "200000"; // 会报错
// 注意点: 其它的用法和JS一样
// money = 0x00;
// money = 0o00;
// money = 0b00;
console.log(money); 

// 布尔类型 boolean
let flag:boolean;
flag = true;
// flag = 1; // 会报错  c语言
console.log(flag);

// 字符串类型 string
let beauty:string;
beauty = "李一桐";
let dream = `我的女神是${beauty},为了她，我想月入${money}k`;
console.log(dream); 


// 总结：
数值，字符串和布尔值是我们开发中最常使用的基础数据类型，与js中的数值，字符串和布尔完全一致，
在ts中我们主要做类型校验使用
```



#### 2.数组

> 数组：声明变量的一组集合称之为数组。

```typescript
export default {}

// 数组类型
// 方式一
// 表示定义了一个名称叫做 beautyList 的数组, 这个数组中将来只能够存储字符串类型的数据
let beautyList:string[]; 
beautyList = ['李嘉欣', '王祖贤', '邱淑贞'];
// arr2 = ['李嘉欣', '王祖贤', '邱淑贞', 200000]; // 报错
console.log(beautyList); 

// 方式二
// 表示定义了一个名称叫做 moneyList 的数组, 这个数组中将来只能够存储数值类型的数据
let moneyList:Array<number>;  
moneyList = [10, 30, 500];
// moneyList = ['a', 30, 500]; // 报错
console.log(moneyList);


// 方式三 联合类型
// 表示定义了一个名称叫做 dream 的数组, 这个数组中将来既可以存储数值类型的数据, 也可以存储字符串类型的数据
let dream:(number | string)[];
dream = [10, 30, 500, "李一桐", "赵露思", "白鹿"];
// dream = [10, 30, 500, "李一桐", "赵露思", "白鹿", false]; // 报错
console.log(dream);


// 方式四 任意类型
// 表示定义了一个名称叫做 arbitrarily 的数组, 这个数组中将来可以存储任意类型的数据
let arbitrarily:any[]; 
arbitrarily = [100, '关之琳', true];
console.log(arbitrarily);

/* 
  数组是我们前端开发过程中，最常用的引用类型之一,在发送请求获取响应时，我们往往会使用到数组类型，
  因此我们务必要掌握好数组的几种定义方式
*/

```

#### 3.元组

- 元祖类型 Tuple
- TS中的元祖类型其实就是数组类型的扩展
- **元组类型用来表示已知元素数量和类型的数组，各元素的类型不必相同，对应位置的类型需要相同**

```tsx
export default {}

// 元祖类型 Tuple
// TS中的元祖类型其实就是数组类型的扩展
// 元组类型用来表示已知元素数量和类型的数组，各元素的类型不必相同，对应位置的类型需要相同

// 表示定义了一个名称叫做 tup1 的元祖, 这个元祖中将来可以存储3个元素, 
// 第一个元素必须是字符串类型, 第二个元素必须是数字类型, 第三个元素必须是布尔类型
let tup1:[string, number, boolean]; 
tup1 = ['宋祖儿', 100, false];
// tup1 = ['宋祖儿', 100, true, 200]; // 超过指定的长度会报错
// tup1 = [100,"宋祖儿", true];
// tup1 = ['杨超越', 100, true];
console.log(tup1); 

/* 
  总结:
  定义: ['', '', ...]
  作用:元祖用于保存定长定数据类型的数据
*/
```

#### 4.any与void

- any:  表示任意类型, 当我们不清楚某个值的具体类型的时候我们就可以使用any

- void:  当一个函数没有返回值时，你通常会见到其返回值类型是 void

```typescript
export default {}

// any类型
// any表示任意类型, 当我们不清楚某个值的具体类型的时候我们就可以使用any
// 在TS中任何数据类型的值都可以负责给any类型

// 使用场景一
// 变量的值会动态改变时，比如来自用户的输入，任意值类型可以让这些变量跳过编译阶段的类型检查
let salary: any = 1800;    // 数字类型
salary = 'my salary is 18k';    // 字符串类型
salary = false;    // 布尔类型

// 使用场景二
// 改写现有代码时，任意值允许在编译时可选择地包含或移除类型检查
let x: any = 4;
x.ifItExists();    // 正确，ifItExists方法在运行时可能存在，但这里并不会检查
x.toFixed();    // 正确 

// 使用场景三
// 定义存储各种类型数据的数组时
let beautyList: any[] = [1, false, 'fine'];
beautyList[1] = 100;



// void类型
// 某种程度上来说，void类型像是与any类型相反，它表示没有任何类型。 
// 当一个函数没有返回值时，你通常会见到其返回值类型是 void
// 在TS中只有null和undefined可以赋值给void类型
function makeMoney(): void {
  console.log("I want to make much money and marry a wife!!!");
  // return "100K beauty" // 报错
}

makeMoney()

let value:void; 
// 定义了一个不可以保存任意类型数据的变量, 只能保存null和undefined
// value = 100; // 报错
// value = "杨紫";// 报错
// value = true;// 报错
// 注意点: null和undefined是所有类型的子类型, 所以我们可以将null和undefined赋值给任意类型
// 严格模式下会null报错
// value = null; // 不会报错  
value = undefined;// 不会报错
```



#### 5.null与undefined

- TypeScript里，`undefined`和`null`两者各自有自己的类型分别叫做`undefined`和`null`。 和 `void`相似，它们的本身的类型用处不是很大
- 非严格模式下，可以把 null和undefined赋值给number类型的变量。

```typescript
export default {}

// TypeScript里，undefined和null两者各自有自己的类型分别叫做undefined和null。 
// 和 void相似，它们的本身的类型用处不是很大

let x: undefined = undefined;
let y: null = null;

let money:number = 100;


// 非严格模式下，可以把 null和undefined赋值给number类型的变量。
money = y;
money = x;

```

#### 6.never与object

> **never类型**: 
>
> 表示的是那些永不存在的值的类型;
>
> never类型是那些总是会抛出异常或根本就不会有返回值的函数表达式或箭头函数表达式的返回值类型; 
>
> 变量也可能是 never类型，当它们被永不为真的类型保护所约束

> **object类型**：
>
> `object`表示非原始类型，也就是除`number`，`string`，`boolean`，`symbol`，`null`或`undefined`之外的类型
>
> 定义了一个只能保存对象的变量
>
> 我们后面更常用的是 `接口` 与 `类型别名`

```typescript
// Never类型
// never类型表示的是那些永不存在的值的类型
// 例如: never类型是那些总是会抛出异常或根本就不会有返回值的函数表达式或箭头函数表达式的返回值类型
//      变量也可能是 never类型，当它们被永不为真的类型保护所约束时。

// 注意点:never类型是任何类型的子类型，也可以赋值给任何类型；然而，没有类型是never的子类型或可以赋值给never类型（除了never本身之外）。
//  即使 any也不可以赋值给never

// 返回never的函数必须存在无法达到的终点
function error(message: string): never {
  throw new Error(message);
}

error("鞠婧祎");

// 推断的返回值类型为never
function fail() {
  return error("Something failed");
}

// 返回never的函数必须存在无法达到的终点
function infiniteLoop(): never {
  while (true) {
  }
}


// Object类型
// 表示一个对象
// 定义了一个只能保存对象的变量
let goddess:object; 
// goddess = 1;
// goddess = "123";
// goddess = true;
goddess = {name:'白鹿', age:27};
console.log(goddess);
```



#### 7.枚举

- `enum`类型是对JavaScript标准数据类型的一个补充。 像C#等其它语言一样，使用枚举类型可以为一组数值赋予友好的名字。

```typescript
// 枚举用于表示固定的几个取值
// 例如: 人的性别只能是男或者女

enum Gender{ 
  Male,
  Femal
}

// 定义了一个名称叫做val的变量, 这个变量中只能保存Male或者Femal
let val:Gender; 
val = Gender.Male;
val = Gender.Femal;
// val = 'nan'; // 报错
// val  = false;// 报错


// 注意点: TS中的枚举底层实现的本质其实就是数值类型, 所以赋值一个数值不会报错
val = 666; // 不会报错
console.log(Gender.Male); // 0
console.log(Gender.Femal);// 1

// 注意点: TS中的枚举类型的取值, 默认是从上至下从0开始递增的
//         虽然默认是从0开始递增的, 但是我们也可以手动的指定枚举的取值的值
// 注意点: 如果手动指定了前面枚举值的取值, 那么后面枚举值的取值会根据前面的值来递增
enum Gender2{ 
  Male=5,
  Femal
}
console.log(Gender2.Male); // 5
console.log(Gender2.Femal);// 6

// 注意点: 如果手动指定了后面枚举值的取值, 那么前面枚举值的取值不会受到影响
enum Gender3{ 
  Male,
  Femal=10
}
console.log(Gender3.Male); // 0
console.log(Gender3.Femal);// 10

// 注意点: 我们还可以同时修改多个枚举值的取值, 如果同时修改了多个, 那么修改的是什么最后就是什么
enum Gender4{ 
  Male=100,
  Femal=200
}
console.log(Gender4.Male); // 100
console.log(Gender4.Femal);// 200

// 我们可以通过枚举值拿到它对应的数字
console.log(Gender.Male); // 0
// 我们还可以通过它对应的数据拿到它的枚举值
console.log(Gender[0]); // Male

```



#### 8.bigint与symbol

```typescript
export default {}

// bigint类型: 表示非常大的数
// symbol类型: 表示全局唯一引用
// ES2020可用

// const Hundred1: bigint = BigInt(100)
// const Hundred2: bigint = 100n

// const firstName = Symbol("name")
// const secondName = Symbol("name")

// if (firstName === secondName) {
//   console.log('ok')
// }
```



#### 9.变量声明与解构

- 与js中完全一致，我们简单复习一下解构
- ‘数组解构与对象解构

```typescript
// 变量声明的方式
// var | let | const

// 数组解构
let goddess = ["邱淑贞", "赵雅芝"];
let [first, second] = goddess;
console.log(first); // 邱淑贞
console.log(second); // 李紫婷



let [third, ...rest] = ["赵今麦", "蒋依依", "欧阳娜娜", "李庚希"];
console.log(third); //  赵今麦
console.log(rest); // ["蒋依依", "欧阳娜娜", "李庚希"];


let [, fourth, , fifth] = [1, 2, 3, 4];
console.log(fourth); // 2
console.log(fifth); // 4



// 对象解构
let beauty  = {
  uname: "杨超越",
  age: 20,
  sex: "女",
}

let { uname, age} = beauty;
console.log(uname);
console.log(age);

```



#### 10.类型断言

- 什么是类型断言?

   类型断言可以用来手动指定一个值的类型，即允许变量从一种类型更改为另一种类型。

   通俗的说就是我相信我自己在定义什么类型

- 语法格式

  `<类型>值`

  `值 as 类型`

```typescript
/*
  1.什么是类型断言?

  类型断言可以用来手动指定一个值的类型，即允许变量从一种类型更改为另一种类型。
  通俗的说就是我相信我自己在定义什么类型

  2.语法格式
    2.1 <类型>值
    2.2 值 as 类型
*/

let str = "世界上最遥远的距离就是,你是if而我是else, 似乎一直相伴但又永远相离";

// 方式一
// let len = (<string>str).length;
// console.log(len);


// 方式二
// let len = (str as string).length;
// console.log(len);



// function TypeAs(x: number | string){
//   console.log(x.length);
  
// }

// TypeAs("世界上最痴心的等待,是我当case你当switch,或许永远都选不上自己")


function TypeAs(x: number | string){
  let len = (x as string).length;
  // let len = (<string>x).length;
  console.log(len);
  
}
TypeAs("世界上最痴心的等待,是我当case而你当switch,或许永远都选不上自己")

```

#### 11.type别名

- 类型别名就是给一个类型起个新名字, 但是它们都代表同一个类型

例如: 你的女神叫邱淑贞, 她的外号叫女神, 女神就是邱淑贞的别名, 邱淑贞和女神都表示同一个人

```typescript
export default {}

// 第一种
type beautys = "邱淑贞" | "赵雅芝" | "王祖贤" | "朱茵"
let one:beautys;

one = "邱淑贞";
// one = 100 // 报错


// 第二种
type myfun = (a:number, b:number) => number;

let fun:myfun = (a:number, b:number) => a + b;
fun(10, 20);


// 第三种
type myGoddass = {
  name: string,
  age: number,
  sex: string,
  actor?: boolean
}

let shuzhen:myGoddass = {
  name: "邱淑贞",
  age: 18,
  sex: "女"
}
```



## 第四章 接口

#### 1.接口的基本使用

- 什么是接口？

  接口是一系列抽象方法的声明，是一些方法特征的集合，这些方法都应该是抽象的，需要由具体的类去实现，

  然后第三方就可以通过这组抽象方法调用，让具体的类执行具体的方法 

- 格式:

`interface interface_name {} `

`type 名称 = {}`

```typescript
/* 
接口是什么？
  接口是一系列抽象方法的声明，是一些方法特征的集合，这些方法都应该是抽象的，需要由具体的类去实现，
  然后第三方就可以通过这组抽象方法调用，让具体的类执行具体的方法 

  接口也是一种数据类型

格式：
  interface interface_name { 
  }
*/

interface IFullName {
  firstName: string
  lastName : string
}

let goddassName: IFullName = {
  firstName: "邱",
  lastName: "淑贞"
}

console.log(goddassName.firstName);
console.log(goddassName.lastName);



function say({firstName, lastName}:IFullName):void {
  console.log(`我的姓名是:${firstName}_${lastName}`);
}

say(goddassName);
```



#### 2.可选属性与只读属性

- 可选属性使用： ？
- 只读属性使用: readonly
- readonly与const的区别: 做为变量使用的话用 const，若做为属性则使用readonly

```typescript
export default {}


// 可选属性   使用?来进行修饰
interface IFullName {
  firstName: string
  lastName : string
  age?: number
}

let goddassName: IFullName = {
  firstName: "邱",
  lastName: "淑贞"
}

console.log(goddassName.firstName);
console.log(goddassName.lastName);


// 只读属性  readonly
interface IInfo {
  readonly uname: string;
  readonly uage: number;
}

let beauty:IInfo = {
  uname: "邱淑贞",
  uage: 18
}

// beauty.uname = "赵丽颖";  // 报错

/* 
  总结: readyonly 与 const 区别:
    最简单判断该用readonly还是const的方法是看要把它做为变量使用还是做为一个属性。 
    做为变量使用的话用 const，
    若做为属性则使用readonly
*/
```



#### 3.索引签名

- 定义: 索引签名用于描述那些“通过索引得到”的类型
- 格式: 如`[props: string]:any`
- 应用场景: 解决参数问题



```typescript
export default {}

interface IFullName {
  firstName: string
  lastName : string
  age?: number
  singName?: string
  [props: string]: any
}

// 注意点: 如果使用接口来限定了变量或者形参, 那么在给变量或者形参赋值的时候,多一个或者少一个都不行
// 实际开发中往往会出现多或者少的情况，怎么解决？


// 少一个或者少多个属性
// 解决方案: 可选属性
let goddass1:IFullName = {firstName: "邱", lastName: "淑贞"};
let goddass2:IFullName = {firstName: "邱", lastName: "淑贞", age: 18};


// 多一个或者多多个属性
// 方案一：使用变量
let info = {firstName: "邱", lastName: "淑贞", age: 18, singName: "赌王", dance: "芭蕾"};
let goddass3:IFullName = info

// 方案二: 使用类型断言
let goddass4:IFullName = ({firstName: "邱", lastName: "淑贞", age: 18, singName: "赌王", dance: "芭蕾"}) as IFullName;


// 索引签名？
// 索引签名用于描述那些“通过索引得到”的类型
// 注意点: 对象中的键，会被转化为字符串
interface Ibeauty {
  [props: string]: string
}

let name:Ibeauty = {name1: "邱淑贞", name2: "李嘉欣", name3: "周慧敏"};

interface Iage {
  [props: string]: number
}

let afe:Iage = {age1: 18, age2: 20};

// 方案三: 索引签名
let goddass5:IFullName = {firstName: "邱", lastName: "淑贞", age: 18, singName: "赌王", dance: "芭蕾"};
```

#### 4.函数接口

- 为了使用接口表示函数类型，我们需要给接口定义一个调用签名。

​       它就像是一个只有参数列表和返回值类型的函数定义。参数列表里的每个参数都需要名字和类型。

```typescript
export default {}

// 函数接口
/* 
  为了使用接口表示函数类型，我们需要给接口定义一个调用签名。 
  它就像是一个只有参数列表和返回值类型的函数定义。参数列表里的每个参数都需要名字和类型
*/

interface ImakeMoney {
  (salary:number, reward:number):number
}


let sum:ImakeMoney = function (x:number, y:number):number {
  return x + y;
}
let res = sum(10, 20);
console.log(res);


// 接口与数组
// 我们定义了StringArray接口，它具有索引签名。 
// 这个索引签名表示了当用 number去索引StringArray时会得到string类型的返回值
interface IStringArray {
  [index: number]: string;
}

let myArray: IStringArray;
myArray = ["邱淑贞", "赵今麦"];

let myStr: string = myArray[1];
console.log(myStr);
```



#### 5.接口的继承

- 接口继承就是说接口可以通过其他接口来扩展自己。
- Typescript 允许接口继承多个接口。
- 继承使用关键字 extends。

```typescript
// 单继承
interface IPerson { 
  age:number 
} 
interface IName extends IPerson { 
  name:string 
} 

let lady:IName = {
  name: "邱淑贞",
  age: 18
}

// 多继承
interface IFatherMoney {
  m1: number
}
interface IMotherMoney {
  m2: number
}

interface ISon extends IFatherMoney, IMotherMoney {
  s: number
} 

let money:ISon = {
  m1: 100,
  m2: 100,
  s: 100
}


console.log(`儿子一共有${money.m1 + money.m2 + money.s}万元`);
```



#### 6.接口的混合类型

- 接口的混合类型就是调用接口的时候，同时包含多种不同的类型
- 应用场景: 闭包

```typescript
export default {}

// 在接口中有多种类型进行混合
interface Counter {
  (start: number): string;
  interval: number;
  reset(): void;
}

function getCounter(): Counter {
  let counter = <Counter>function (start: number) { };
  counter.interval = 123;
  counter.reset = function() { };

  return counter;
}

let c = getCounter();
c(10);
c.reset();
c.interval = 5.0;
```



#### 7.接口与类型别名的异同

> 1.相同点:
>
> - 都可以描述属性或方法
> - 都允许拓展
>
> 2.不同点:
>
> - type可以声明基本数据类型，联合类型，数组等; interface只能声明变量
> - 当出现使用type和interface声明同名的数据时;type会直接报错;interface会进行组合
> - type不会自动合并；interface会

```typescript
export default {}

// 相同点:
// 1.都可以描述属性或方法
type womenStar = {
  name: string
  age: number
  perform(): any
}
interface IWStar {
  name: string
  age: number
  perform(): any
}

let star1 = {
  name: "邱淑贞",
  age: 18,
  perform() {
    return "倚天屠龙记"
  }
}
let star2 = {
  name: "李一桐",
  age: 18,
  perform() {
    return "射雕英雄传"
  }
}

// 2.都允许拓展
type money  = {
  y1: number
}
type money2 = money & {
  y2: number
}

let salary:money2 = {
  y1: 10,
  y2: 20
}

interface Istar1 {
  name: string
}
interface Istar2 extends Istar1 {
  age: number
}

let starInfo:Istar2 = {
  name: "邱淑贞",
  age: 18
}


// 不同点：
// 1.type可以声明基本数据类型，联合类型，数组等
//   interface只能声明变量
type age = number;
type info = string | number | boolean;
type beautyList = [string | number];
// interface Iage = number; // 报错


// 2.当出现使用type和interface声明同名的数据时
//   type会直接报错
//   interface会进行组合
// type mygoddassName = {
//   name: string
// }

// type mygoddassName = {
//   name: number
// }

interface mygoddassName {
  name: string
} 
interface mygoddassName {
  name: string
  age: number
} 

let goddass:mygoddassName = {
  name: "赵丽颖",
  age: 20
}
```



## 第五章 函数

#### 1.函数的基本使用

- 介绍

  函数是JavaScript应用程序的基础。 它帮助你实现抽象层，模拟类，信息隐藏和模块。 在TypeScript里，虽然已经支持类，命名空间和模块，但函数仍然是主要的定义 *行为*的地方。 TypeScript为JavaScript函数添加了额外的功能，让我们可以更容易地使用



- 函数定义的方式

```typescript
export default {}

// 匿名函数
const makeMoney = function(salary: number, reward: number): number {
  return salary + reward
}


// 有名函数 | 命名函数 | 普通函数
function writeCode(hour: number, sleep: number) {
  return hour
}
 

// 箭头函数
const seeMeiMei = (time: number):void => {
  console.log(`我每天要看${time}个小时MeiMei`);
  
}

seeMeiMei(8)


// 接口函数
type myFunc = (x: number, y: number) => number

const myfunc:myFunc = (a: number, b: number) => a + b
```

#### 2.函数参数的处理

- 可选参数: 

  在 TypeScript 函数里，如果我们定义了参数，则我们必须传入这些参数，除非将这些参数设置为可选，可选参数使用问号标识  `？`

- 默认参数:

  我们也可以设置参数的默认值，这样在调用函数的时候，如果不传入该参数的值，则使用默认参数，语法格式为 ``

  ```
  function function_name(param1[:type],param2[:type] = default_value) { 
  }
  ```

- 剩余参数:

  有一种情况，我们不知道要向函数传入多少个参数，这时候我们就可以使用剩余参数来定义。

  剩余参数语法允许我们将一个不确定数量的参数作为一个数组传入。`...args:any[]`

```typescript
export default {}


// 可选参数
const func1:(x: number, y?: number)=>number = function(a, b) {
  return a;
}


const func2 = function(a: number, b?: number): number {
  return a;
}

func2(10);
func2(10, 20);
func2(10, undefined);


// 函数的默认值
const func3 = function(a: number = 1, b:number =2, c:number=3) {
  return a + b + c;
}

func3();
func3(10);
func3(10, 20);
func3(10, 20, 30);


// 函数的剩余参数
const func4 = function(...args:any[]) {
  console.log(args);
  
}

func4(10, 20 , 30, "邱淑贞");

const func5 = function(a:number, b:number, ...args:any[]) {
  console.log(a);
  console.log(b);
  console.log(args);
  
}

func5(10, 20 , 30, "邱淑贞", "邢菲");
```

#### 3.构造函数

TypeScript 也支持使用 JavaScript 内置的构造函数 Function() 来定义函数：

语法格式如下：

```typescript
var res = new Function ([arg1[, arg2[, ...argN]],] functionBody)
```

参数说明：

- **arg1, arg2, ... argN**：参数列表
- **functionBody**：一个含有包括函数定义的 JavaScript 语句的字符串。



```typescript
export default {}


// 构造函数
var myFunction = new Function("a", "b", "return a * b"); 
var x = myFunction(4, 3); 
console.log(x);


// 递归函数
function sum(arr: number[], n: number):number {
  if(n <= 0){
    return 0;
  }else {
    return sum(arr, n-1) + arr[n-1];
  }
}

sum([2, 3, 4, 5], 3)

```

#### 4.函数重载

重载是方法名字相同，而参数不同，返回类型可以相同也可以不同。

每个重载的方法（或者构造函数）都必须有一个独一无二的参数类型列表。

**参数类型不同：**

> ```
> function disp(string):void; 
> function disp(number):void;
> ```

**参数数量不同：**

> ```
> function disp(n1:number):void; 
> function disp(x:number,y:number):void;
> ```

**参数类型顺序不同：**

> ```
> function disp(n1:number,s1:string):void; 
> function disp(s:string,n:number):void;
> ```

如果参数类型不同，则参数类型应设置为 **any**。

参数数量不同你可以将不同的参数设置为可选。

```typescript
export default {}


// 不使用函数重载的问题
function add(a: number, b: number){
  return a + b;
}

add(10, 20);


function add2(a: string, b: string){
  return a + b;
}

add2("我的女神是: ", "邱淑贞");



function add3(a: string| number, b: string | number){
  // return a + b;
  if( typeof a=="number" && typeof b=="number"){
    return a + b;
  }
  if( typeof a=="string" && typeof b=="string"){
    return a + b;
  }
  if( typeof a=="string" && typeof b=="number"){
    return a + b;
  }
  if( typeof a=="number" && typeof b=="string"){
    return a + b;
  }
}

add3("我的女神是: ", "邱淑贞");
add3(10, 20);
add3("邱淑贞", 20);


// 定义函数重载
function addFunc(a:number, b: number):number;
function addFunc(a:string, b: string):string;
function addFunc(a:number, b: string):string;
function addFunc(a:string, b: number):string;


// 使用函数重载
function addFunc(a: any, b: any):any {
  return a + b;
}
addFunc(10, 20);
addFunc("谭松韵", "金晨");
addFunc(27, "白鹿");
addFunc("赵今麦", 19);


// 定义参数类型与参数数量不同
function star(s1:string):void; 
function star(n1:number,s1:string):void; 
 
function star(x:any,y?:any):void { 
    console.log(x); 
    console.log(y); 
} 
star("王心凌"); 
star(1,"爱你");
```



#### 5.this的使用

- JavaScript里，`this`的值在函数被调用的时候才会指定。 这是个既强大又灵活的特点，但是你需要花点时间弄清楚函数调用的上下文是什么。 但众所周知，这不是一件很简单的事，尤其是在返回一个函数或将函数当做参数传递的时候。
- 从 TypeScript 2.0 开始，在函数和方法中我们可以声明 `this` 的类型，实际使用起来也很简单
  - 使用this参数，改变指向
  - 传入this参数，禁止调用this



```typescript
export default {};

let userInfo = {
  name: "邱淑贞",
  age: 18,
  song: "恨你太无情",
  marry: true,
  show: function () {
    this.marry = false;
  },
};



class Rectangle1 {
  private w: number;
  private h: number;

  constructor(w: number, h: number) {
    this.w = w;
    this.h = h;
  }

  getArea() {
    return () => {
      return this.w * this.h;
    };
  }
}

class Rectangle2 {
  private w: number;
  private h: number;

  constructor(w: number, h: number) {
    this.w = w;
    this.h = h;
  }

  getArea(this: Rectangle2) {
    return () => {
      return this.w * this.h;
    };
  }
}

// class Rectangle3 {
//   private w: number;
//   private h: number;

//   constructor(w: number, h: number) {
//     this.w = w;
//     this.h = h;
//   }

//   getArea(this: void) {
//     return () => {
//       return this.w * this.h;
//     };
//   }
// }

```

#### 6.特殊的函数返回值

- 如果使用类型别名，定义了一个返回值为void的类型, 我们在函数使用的时候，并非一定不能有返回值。

  相反，如果我们在函数中写了返回值，我们的返回值是有效的。

- 如果我们定义函数的时候明确指出，返回值为void，那么我们将除undefined 和 null 以外的值进行返回都会进行报错



```typescript
export default {}


type voidFunc = () => void


let func1: voidFunc = function() {
  return true;
}


let func2: voidFunc = () => {
  return false;
}

let f1 = func1();
let f2 = func2();
console.log("f1: ", f1);
console.log("f2: ", f2);



// 注意点: 如果我们定义函数的时候明确指出，返回值为void，
// 那么我们将除undefined 和 null 以外的值进行返回


function func3():void {
  // return 123 // 报错
}
```

## 第六章 类的使用

#### 1.类的基本使用

- 定义

  TypeScript 是面向对象的 JavaScript。

  类描述了所创建的对象共同的属性和方法。

  TypeScript 支持面向对象的所有特性，比如 类、接口等。

  TypeScript 类定义方式如下:

> ```
> class class_name { 
>  // 类作用域
> }
> ```

定义类的关键字为 class，后面紧跟类名，类可以包含以下几个模块（类的数据成员）：

- **字段** − 字段是类里面声明的变量。字段表示对象的有关数据。
- **构造函数** − 类实例化时调用，可以为类的对象分配内存。
- **方法** − 方法为对象要执行的操作。

```typescript
class Person {
  // 注意点: 需要先定义实例属性，才能够使用
  name: string
  age: number

  constructor(name: string, age: number){
    this.name = name;
    this.age = age;
  }

  sayHello(): void{
    console.log(`我的女神是${this.name}, 她今年${this.age}岁了, 但是在我心里她永远18岁!`);
  }

}

let p = new Person("邱淑贞", 54);
p.sayHello();
```

#### 2.类的继承

TypeScript 支持继承类，即我们可以在创建类的时候继承一个已存在的类，这个已存在的类称为父类，继承它的类称为子类。

类继承使用关键字 **extends**，子类除了不能继承父类的私有成员(方法和属性)和构造函数，其他的都可以继承。

TypeScript 一次只能继承一个类，不支持继承多个类，但 TypeScript 支持多重继承（A 继承 B，B 继承 C）。

语法格式如下：`class child_class_name extends parent_class_name`

```typescript
export default {}


class Person {
  name: string
  age: number

  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }

  say():void{
    console.log(`我是${this.name}, 今年${this.age}岁`);
    
  }
}


class Student extends Person {
  score: string;

  constructor(name: string, age: number, score: string) {
    super(name, age);
    this.score = score;
  }

  say():void {
    // 调用父类的函数
    super.say();
    console.log(`我是重写之后的say方法, 我是学生${this.name}, 今年${this.age}岁了, 我的成绩为${this.score}`);
  }
}

let s = new Student("蒋依依", 18, "A");
s.say();


```

#### 3.static与instanceof

- static 关键字用于定义类的数据成员（属性和方法）为静态的，静态成员可以直接通过类名调用。

- instanceof 运算符用于判断对象是否是指定的类型，如果是返回 true，否则返回 false。

```typescript
export default {}

// static关键字
// static 关键字用于定义类的数据成员（属性和方法）为静态的，静态成员可以直接通过类名调用。
class StaticTest {
  static salary: string;

  static say(): void {
    console.log("我们想要的工资是: " + StaticTest.salary);
    
  }
}

StaticTest.salary = "18k";
StaticTest.say();


// instanceof运算符
// instanceof 运算符用于判断对象是否是指定的类型，如果是返回 true，否则返回 false。
class Person{} 
let p = new Person() 
let isPerson = p instanceof Person; 
console.log("p 对象是 Person 类实例化来的吗？ " + isPerson); // true

class Student extends Person {}
let s = new Person() 
let isPerson2 = s instanceof Person; 
console.log("s 对象是 Person 类实例化来的吗？ " + isPerson2); // true
```

#### 4.类中的修饰符

- **public(默认)**：公有，可以在任何地方被访问
- **protected**:  受保护，可以被其自身以及其子类访问
- **private:**  私有，只能被其定义所在的类访问。
- **readonly**: 可以使用 `readonly`关键字将属性设置为只读的。 只读属性必须在声明时或构造函数里被初始化。

```typescript
export default {}

class Person {
  public name: string;
  protected age: number;
  private sex: string;

  constructor(name: string, age: number, sex: string) {
    this.name = name;
    this.age = age;
    this.sex = sex;
  }

  say():void {
    console.log(`我的名字是${this.name},性别为${this.sex}, 今年${this.age}岁了,`);
  }
}

class Student extends Person {
  score: string
  constructor(name: string, age: number, sex: string, score: string) {
    super(name, age, sex);
    this.score = score;
  }
  show():void {
    console.log(this.name);
    console.log(this.age);
    // console.log(this.sex);
    console.log(this.score);
    
  }
}

let p = new Person("邱淑贞", 18, "女");
p.say();

let s = new Student("王心凌", 18, "女", "A");
s.show();

// 思考题: 如果我们给 constructor 加上 protected 会出现什么情况？


// readonly: 字段的前缀可以是 readonly 修饰符。这可以防止在构造函数之外对该字段进行赋值。
class PrintConsole {
  readonly str1: string = "HTML, CSS, JS, VUE REACT, NODE"
  readonly str2: string;
  readonly str3: string;
  readonly str4: string;

  constructor(str2: string, str3:string, str4:string) {
    this.str2 = str2;
    this.str3 = str3;
    this.str4 = str4;
  }
  // show():void {
  //   this.str2 = "123"
  // }
}

let pc = new PrintConsole("我的头发去哪了, 颈椎康复指南", 
                          "35岁失业该怎么办, 外卖月入一万也挺好", 
                          "活着") 

```

#### 5.getter与setter

官方的另外一个名字: `存取器`

通过getters/setters来截取对对象成员的访问

**注意点:**

> 如果存在 get ，但没有 set ，则该属性自动是只读的
> 如果没有指定 setter 参数的类型，它将从 getter 的返回类型中推断出来
> 访问器和设置器必须有相同的成员可见性

```typescript
export default {}

class GetNameClass {
  private _fullName: string = "倪妮"

  get fullName():string {
    console.log("我是get方法");
    return this._fullName
  }

  set fullName(newName:string) {
    console.log("我是set方法");
    this._fullName = newName;
  }
}


let starname = new GetNameClass();
starname.fullName = "袁冰妍"

console.log(starname);

console.log(starname.fullName);

```

#### 6.抽象类

- 定义

  抽象类做为其它派生类的基类使用。 它们一般不会直接被实例化

  抽象类是专门用于定义哪些不希望被外界直接创建的类的

  抽象类和接口一样用于约束子类

- 抽象类和接口区别

  抽象方法必须包含 `abstract`关键字并且可以包含访问修饰符

  接口中只能定义约束, 不能定义具体实现。而抽象类中既可以定义约束, 又可以定义具体实现

```typescript
export default {}

abstract class Person {
  abstract name: string;
  abstract show(): string;

  showName() {
    console.log(this.show());
  }
}


class Student extends Person {
  name: string = "孟子义";
  show():string {
    return "陈情令"
  }
}


// let p = new Person();
let s = new Student();
let res =  s.show();
console.log(res);
```

#### 7.implements子句

- 类可以实现接口，使用关键字 implements
- 可以使用一个 implements 子句来检查一个类，是否满足了一个特定的接口。如果一个类不能正确地
  实现它，就会发出一个错误

注意点: 

​	实现一个带有可选属性的接口并不能创建该属性

​	只要一个接口继承了某个类, 那么就会继承这个类中所有的属性和方法,但是只会继承属性和方法的声明, 不会继承属	性和方法实现

与`extends`的区别

>extends: 继承某个类，继承之后可以使用父类的方法，也可以重写父类的方法
>
>implements:继承某个类，**必须重写**才可以使用

```typescript
export default {}

/* 
  extend: 继承某个类，继承之后可以使用父类的方法，也可以重写父类的方法
  implements:继承某个类，必须重写才可以使用
*/

interface IPersonInfo {
  name: string;
  age: number;
  sex?: string; 
  show(): void;
}

interface IMusic {
  music: string
}

class Person implements IPersonInfo, IMusic {
  name: string = "吴谨言";
  age: number = 32;
  music: string = "雪落下的声音";
  show() {
    console.log(`${this.name}是'延禧攻略'的主演，她今年${this.age}岁了`);
    console.log(`${this.name}唱了一首歌叫 ${this.music}`);
    
  }
}
let p = new Person();
p.show();
// p.name = "周冬雨"
// p.sex = "女" // 报错


// 注意点: 只要一个接口继承了某个类, 那么就会继承这个类中所有的属性和方法
// 但是只会继承属性和方法的声明, 不会继承属性和方法实现

interface ITest extends Person {
  salary: number
}

class Star extends Person implements ITest {
  salary: number = 50;
  name: string = "关晓彤";
  age: number = 18;
}


let s = new Star();
console.log(s.salary);
console.log(s.name);

```

#### 8.类的初始化顺序

- 基类的字段被初始化
- 基类构造函数运行
- 子类的字段被初始化
- 子类构造函数运行

```typescript
export default {}

/* 
  1.基类的字段被初始化
  2.基类构造函数运行
  3.子类的字段被初始化
  4.子类构造函数运行
*/

class Old {
  name: string = "林青霞"
  constructor() {
    console.log("我的名字是：" + this.name);
  }
}

class Young extends Old {
  name: string = "李子璇"
  constructor () {
    super()
    // console.log(this.name);
  }
}

let y = new Young();

```

## 第七章 泛型

#### 1.泛型的基本使用

- 泛型可以理解为宽泛的类型，通常用于类和函数。使用的时候我们再指定类型
- 泛型不仅可以让我们的代码变得更加健壮, 还能让我们的代码在变得健壮的同时保持灵活性和可重用性
- 通过用 `<T>`来表示，放在参数的前面

```typescript
export default {}

// 不使用泛型
// let getArray = (value:number, items:number):number[]=>{
//     return new Array(items).fill(value);
// };
// let arr = getArray(8, 3);
// // let arr = getArray("abc", 3); // 报错
// console.log(arr);


// let getArray = (value:any, items:number):any[]=>{
//     return new Array(items).fill(value);
// };

// let arr = getArray("刘亦菲", 10)
// // let arr = getArray(10, 10)
// console.log(arr);
// let res = arr.map(item=>item.length); 
// console.log(res);



// 使用泛型
let getArray = <T>(value:T, items:number):T[]=>{
    return new Array(items).fill(value);
};

let arr = getArray<string>("刘亦菲", 3)
// let arr = getArray<number>(10, 3)

let res = arr.map(item => item.length);
console.log(res);

// 回顾
const techArr1: string[] = ["HTML", "CSS", "JS"];
const techArr2: Array<String> = ["VUE", "REACT", "ANGULAR"];
```

#### 2.泛型约束

- 在TS中，我们需要严格的设置各种类型，我们使用泛型之后，将会变得更加灵活，但同时也将会存在一些问题
- 我们需要对泛型进行约束来解决这些问题

```typescript
export default {}


// 演示可能出现的问题
// function getLength<T>(arr: T): T{
//   console.log(arr.length);
//   return arr;
// }



// 通用的方法
// function getLength<T>(arr: Array<T>): Array<T> {
//   console.log(arr.length); 
//   return arr;
//  }
 

// 泛型接口
interface ILength {
  length: number
}

function getLength<T extends ILength>(arr: T): number {
  return arr.length
}

getLength("孟子义");
getLength([1, 2, 3]);
getLength({length: 20});
```

#### 3.泛型接口

- 将泛型与接口结合起来使用，可以大大简化我们的代码，增加我们的代码可读性
- 泛型也可以使用默认值

```typescript
export default {}


// interface IPerson {
//   name: string
//   sex: string
// }

// let p: IPerson = {
//   name: "于文文",
//   sex: "女"
// }

// interface IPerson<T1, T2> {
//   name: T1
//   sex: T2
// }

// let p: IPerson<String, number> = {
//   name: "于文文",
//   sex: 0
// }


interface IPerson<T1=String, T2=number> {
  name: T1
  sex: T2
}

let p: IPerson = {
  name: "于文文",
  sex: 0
}
```

#### 4.泛型类

- 泛型类看上去与泛型接口差不多。 泛型类使用（ `<>`）括起泛型类型，跟在类名后面。

```typescript
export default {}

class Person<T1, T2> {
  name: T1
  age: T2
  sex: T1

  constructor(name: T1, age: T2, sex: T1) {
    this.name = name
    this.age = age
    this.sex = sex
  }
}

const p1 = new Person("刘诗诗", 18, "女")
const p2 = new Person<String, number>("虞书欣", 18, "女")
const p3:Person<String, number> = new Person("刘诗诗", 18, "女")
```

#### 5.使用类型参数进行约束

- 一个泛型被另一个泛型约束, 就叫做泛型约束中使用类型参数
- 你可以声明一个类型参数，且它被另一个类型参数所约束。 比如，现在我们想要用属性名从对象里获取这个属性。并且我们想要确保这个属性存在于对象 obj上，因此我们需要在这两个类型之间使用约束

```typescript
export default {}

// 在泛型约束中使用类型参数
// 你可以声明一个类型参数，且它被另一个类型参数所约束。 比如，现在我们想要用属性名从对象里获取这个属性。
// 并且我们想要确保这个属性存在于对象 obj上，因此我们需要在这两个类型之间使用约束

// interface IkeyInterface {
//   [key: string]: any
// }

// let getProps = (obj:IkeyInterface, key:string): any => {
//   return obj[key]
// } 

// let x ={ a: 1, b:2 };
// let res = getProps(x, "a");
// // let res = getProps(x, "c"); // 没报错
// console.log(res);


function getProperty<T, K extends keyof T>(obj: T, key: K) {
  return obj[key];
}

let x = { a: 1, b: 2};
getProperty(x, "a"); 
// getProperty(x, "c");  // 报错
```

## 第八章 一些补充

#### 1.unknown类型

-  unknown 类型代表任何值。这与 any 类型类似，但更安全，因为对未知 unknown 值做任何事情都是不合法的。
- unknown 类型被称作安全的any

```typescript
export default {}

// unknown 类型代表任何值。这与 any 类型类似，但更安全，因为对未知 unknown 值做任何事情都是不合法的。
// unknown 类型被称作安全的any

// 1.任何类型都可以赋值给unknown类型
let str:unknown;
str = 18;
str = "张馨予";
str = false;

// 2.不能将unknown类型赋值给其它类型
let val:unknown = 18;
let num: number;
// num = val; // 报错
// 使用类型断言
num = val as number;
// 使用类型缩小
if(typeof val == "number") {
  num = val;
}

// 3.unknown与其它任何类型组成的交叉类型最后都是其它类型
type MyType1 = number & unknown;
type MyType2 = unknown & boolean;
let a:MyType1 = 18;
let b:MyType2 = true;


// 4.unknown除了与any以外, 与其它任何类型组成的联合类型最后都是unknown类型
type MyType3 = unknown | any;
type MyType4 = unknown | number;
type MyType5 = unknown | string | boolean;


// 5.never类型是unknown类型的子类型
type MyType6 = never extends unknown ? true : false;

```

#### 2.map类型

Map 对象保存键值对，并且能够记住键的原始插入顺序。

任何值(对象或者原始值) 都可以作为一个键或一个值。

Map 是 ES6 中引入的一种新的数据结构

可以使用for of 进行迭代

**创建map:**

> ```typescript
> let myMap = new Map();
> ```

**Map 相关的函数与属性：**

 - **map.clear()** – 移除 Map 对象的所有键/值对 。
 - **map.set()** – 设置键值对，返回该 Map 对象。
 - **map.get()** – 返回键对应的值，如果不存在，则返回 undefined。
 - **map.has()** – 返回一个布尔值，用于判断 Map 中是否包含键对应的值。
 - **map.delete()** – 删除 Map 中的元素，删除成功返回 true，失败返回 false。
 - **map.size** – 返回 Map 对象键/值对的数量。
 - **map.keys()** - 返回一个 Iterator 对象， 包含了 Map 对象中每个元素的键 。
 - **map.values()** – 返回一个新的Iterator对象，包含了Map对象中每个元素的值 。

```typescript
let nameSiteMapping = new Map();
 
// 设置 Map 对象
nameSiteMapping.set("邱淑贞", 1);
nameSiteMapping.set("宋茜", 2);
nameSiteMapping.set("景甜", 3);
 
// 获取键对应的值
console.log(nameSiteMapping.get("Runoob"));     // 2
 
// 判断 Map 中是否包含键对应的值
console.log(nameSiteMapping.has("Taobao"));       // true
console.log(nameSiteMapping.has("Zhihu"));        // false
 
// 返回 Map 对象键/值对的数量
console.log(nameSiteMapping.size);                // 3
 
// 删除 Runoob
console.log(nameSiteMapping.delete("Runoob"));    // true
console.log(nameSiteMapping);
// 移除 Map 对象的所有键/值对
nameSiteMapping.clear();             // 清除 Map
console.log(nameSiteMapping);

// 迭代 Map 中的 key
for (let key of nameSiteMapping.keys()) {
    console.log(key);
}
// 迭代 Map 中的 value
for (let value of nameSiteMapping.values()) {
    console.log(value);
}
// 迭代 Map 中的 key => value
for (let entry of nameSiteMapping.entries()) {
    console.log(entry[0], entry[1]);
}
// 使用对象解析
for (let [key, value] of nameSiteMapping) {
    console.log(key, value);
}
```

#### 3.索引类型

- 我们可以使用一个索引访问类型来查询另一个类型上的特定属性
- 格式: [ key]
- 注意点: 不会返回null/undefined/never类型

```typescript
export default {}

// 我们可以使用一个索引访问类型来查询另一个类型上的特定属性
// 格式: []

class Person {
  name: string 
  age: number
}

type MyIndex = Person["name"];
let a:MyIndex = "赵韩樱子";
console.log(a);

//  获取指定对象, 部分属性的值, 放到数组中返回
let obj = {
  name:'吴倩',
  age:18,
  gender: true
}
function getValues<T, K extends keyof T>(obj:T, keys:K[]):T[K][] {
  let arr = [] as T[K][];
  keys.forEach(key=>{
      arr.push(obj[key]);
  })
  return arr;
}
let res = getValues(obj, ['name', 'age']);
console.log(res);

// 不会返回null/undefined/never类型
interface TestInterface {
    a:string,
    b:number,
    c:boolean,
    d:symbol,
    e:null,
    f:undefined,
    g:never
}
type MyType = TestInterface[keyof TestInterface];

```

#### 4.条件类型

- 条件类型的形式看起来有点像JavaScript中的条件表达式
- T extends U ? TrueType : FalseType
- 应用场景: 解决函数重载问题

```typescript
export default {}

// 1.条件类型
// 条件类型的形式看起来有点像JavaScript中的条件表达式
// T extends U ? TrueType : FalseType

interface IName {
  name: string
}

interface IAge {
  age: number
}

// function reLoad(name: string): IName;
// function reLoad(age: number): IAge;
// function reLoad(nameOrAge: string | number): IName | IAge;
// function reLoad(nameOrAge: string | number): IName | IAge {
//   throw ""
// }

// 条件类型
type MyType<T> = T extends string ?: string : any
type res = MyType<string>


// 2.应用场景：解决函数重载问题
type Condition<T> = T extends string ? IName : IAge


function reLoad<T extends number | string>(idOrName: T): Condition<T> {
  throw ""
}

let res1 = reLoad("王丽坤") 
let res2 = reLoad(100) 
```

#### 5.分布式条件类型

- 当条件类型作用于一个通用类型时，当给定一个联合类型时，它们就变成了分布式的
- 通常情况下，分布性是需要的行为。为了避免这种行为，你可以用方括号包围 extends 关键字的每一
  边。

```typescript
export default {}

// 定义：被检测类型是一个联合类型的时候, 该条件类型就被称之为分布式条件类型

// type MyType<T> = T extends any ? T : never;
// type res = MyType<string | number | boolean>;


// 从T中剔除可以赋值给U的类型。 Exclude
// type res = Exclude<string | number | boolean, number>


// 提取T中可以赋值给U的类型。 Extract
// type res = Extract<string | number | boolean, number>

// 从T中剔除null和undefined。 NonNullable
// type res = NonNullable<string | boolean | null | undefined>

// 获取函数返回值类型。 ReturnType
// type res = ReturnType<()=>number>

// 获取一个类的构造函数参数组成的元组类型。 ConstructorParameters
// class Person {
//     constructor(name:string, age:number){}
// }
// type res = ConstructorParameters<typeof Person>;


// 获得函数的参数类型组成的元组类型。 Parameters
// function say(name:string, age:number, gender:boolean) {
// }
// type res = Parameters<typeof say>;
```

#### 6.infer关键字推断

- 条件类型为我们提供了一种方法来推断我们在真实分支中使用 infer 关键字进行对比的类型
- 我们可以使用 infer 关键字编写一些有用的辅助类型别名

```typescript
export default {}

// 假如想获取数组里的元素类型，如果是数组则返回数组中元素的类型，否则返回这个类型本身
type Id = number[];
type IName = string[];

type Unpacked<T> = T extends IName ? string : T extends Id ? number : T;

type idType = Unpacked<Id>; // idType 类型为 number
type nameType = Unpacked<IName>; // nameType 类型为string


// 使用infer简化操作
type ElementOf<T> = T extends Array<infer E> ? E : T;
type res1 = ElementOf<string[]>; // string
type res2 = ElementOf<boolean>; // boolean


// infer可以推断出联合类型
type Foo<T> = T extends { a: infer U; b: infer U } ? U : never;
type T11 = Foo<{ a: string; b: number }>; // T11类型为 string | number

```

#### 7.映射类型

- 当你不想重复定义类型，一个类型可以以另一个类型为基础创建新类型。通俗的说就是，以一个类型为基础，根据它推断出新的类型
- Readonly / Partial 关键字
- Record / Pick 映射类型
- Readonly， Partial和 Pick是同态的，但 Record不是。 因为 Record并不需要输入类型来拷贝属性，所以它不属于同态

```typescript
export default {}

// 当你不想重复定义类型，一个类型可以以另一个类型为基础创建新类型。
// 通俗的说就是，以一个类型为基础，根据它推断出新的类型

interface IPerson {
  name: string;
  age: number;
}

// 只读
type Readonly1<T> = {
  readonly [P in keyof T]: T[P];
}

type ReadonlyRes = Readonly1<IPerson>;

// 可选
type Partial1<T> = {
  [P in keyof T]?: T[P];
}

type PartialRes = Partial1<IPerson>;


// 通过 + - 指定添加或者删除
interface IPerson2 {
  readonly name?: string;
  readonly age?: number;
}

type ReadonlyPartial<T> = {
  -readonly [P in keyof T]-?: T[P]
}

type res = ReadonlyPartial<IPerson2>;


// Readonly/Partial 关键字
interface IPerson3 {
  name: string;
  age: number;
}

type Readonly<T> = {
  readonly [P in keyof T]: T[P];
}
type Partial<T> = {
  [P in keyof T]?: T[P];
}

type res1 = Readonly<IPerson3>;
type res2 = Partial<IPerson3>;
```

```typescript
export default {}

// Record映射类型
// 他会将一个类型的所有属性值都映射到另一个类型上并创造一个新的类型
type Name = "person" | "animal" ;
type Person = {
  name: string
  age: number
}
// interface IPerson  {
//   name: string
//   age: number
// }

type NewType = Record<Name, Person>
let res: NewType = {
  person: {
    name: "唐艺昕",
    age: 18
  },
  animal: {
    name: "云梦",
    age: 0.3
  }
}
console.log(res);


// Pick映射类型
// 将原有类型中的部分内容映射到新类型中
// type Info = {
//   name: string
//   age: number
// }
interface IInfo  {
  name: string
  age: number
}

type PartProp = Pick<IInfo, "name">
let res2:PartProp = {
  name: "韩雪"
}

```

#### 8.其他公共类型

- Required<Type>

  构建一个由 Type 的所有属性组成的类型，设置为必填。与 Partial 相反

- Omit<Type, Keys>

  通过从 Type 中选取所有属性，然后删除 Keys （属性名或属性名的联合）来构造一个类型。

- OmitThisParameter

  从类型T中剔除this参数类型，并构造一个新类型

```typescript
export default {}

// Required<Type>
// 构建一个由 Type 的所有属性组成的类型，设置为必填。与 Partial 相反
interface IPerson {
  name?: string;
  age?: number;
}

let res:IPerson = {
  name: "舒畅",
}

let res2: Required<IPerson> = {
  name: "舒畅",
  age: 18
}

// Omit<Type, Keys> 
// 通过从 Type 中选取所有属性，然后删除 Keys （属性名或属性名的联合）来构造一个类型。
interface Student {
  name: string;
  age: number
  score: number;
  sex: boolean;
}
 
type SProps = Omit<Student, "name">

let res3:SProps = {
  age: 18,
  score: 100,
  sex: false
}


// OmitThisParameter
// 从类型T中剔除this参数类型，并构造一个新类型

function add(x: number): void {
  console.log(x)
}

function f0(this: object, x: number) {}
function f1(x: number) {}

// (x: number) => void
type T0 = OmitThisParameter<typeof f0>;

// (x: number) => void
type T1 = OmitThisParameter<typeof f1>;

// string
type T2 = OmitThisParameter<string>;

const x: T0 = add;
const y: T1 = add;
const z: T2 = "江疏影";

console.log(x)
console.log(y)
console.log(z)
```

## 第九章     TS中的兼容性

#### 1.自动类型推论

- 根据初始值进行类型推论

  不用明确告诉编译器具体是什么类型, 编译器就知道是什么类型

  根据初始化值自动推断

  **注意点：**  如果是先定义在初始化, 那么是无法自动推断的

- 上下文类型推论

  TypeScript类型推论也可能按照相反的方向进行。 这被叫做“按上下文归类”。按上下文归类会发生在表达式的类型与所处的位置相关时

```typescript
export default {}

// 最佳类型推断
// 等价于 let uanme: string = "陈乔恩";
let uname = "陈乔恩"; 
uname = "徐璐"
// uname = 123;  // 报错
// uname = true; // 报错

let uage;
uage = 123;
uage = true;

// 等价于 let x: (number | null)[] = [0, 1, null];
let x = [0, 1, null];
// x = [18, 28, 38, null, true];


// 上下文类型推断
window.onmousedown = function (mouseEvent) {
  console.log(mouseEvent.button);
};
 
```

#### 2.对象类型兼容性

- 可多不可少
- 会进行递归检查

```typescript
export default {}


// 对象类型赋值给接口类型
// 注意点: 可多不可少
interface INameTest {
  name: string;
}

let n1 = {name: "祝绪丹"};
let n2 = {name: "江疏影", age: 18};
let n3 = {age: 18};

let val: INameTest;
val = n1;
val = n2;
// val = n3;



// 注意点: 必须一一对应，会进行递归检查
interface ITestInfo {
  name:string;
  children:{
      age:number
  };
}

let p1 = {name:'吴宣仪', children:{age:18}};
let p2 = {name:'陈小纭',children:{age:true}};

let t:ITestInfo;
t = p1;
// t = p2; 
```

#### 3.函数类型兼容性

- 参数个数
- 参数类型
- 参数返回值
- 双向协变
- 函数重载
- 可选参数及剩余参数

```typescript
export default {}


// 参数个数
// 注意点: 可少不可多
/*
let func1 = (a:number, b:number) => {};
let func2 = (x:number) => {};
func1 = func2;
func2 = func1; 
*/



// 参数类型
// 注意点: 参数类型必须相同
/*
let func1 = (x:number)=>{};
let func2 = (x:number)=>{};
let func3 = (x:string)=>{};
func1 = func2;
func2 = func1;
func1 = func3; 
func3 = func1;
*/

// 返回值类型
// 注意点: 返回值类型必须相同
/*
let func1 = ():number=> 18;
let func2 = ():number=> 28;
let func3 = ():string=> 'TS真好玩';
func1 = func2;
func2 = func1;
func1 = func3; 
func3 = func1;
*/


// 函数双向协变
// 1.参数的双向协变
/*
let func1 = (x:(number | string)) =>{};
let func2 = (x:number) =>{};
func1 = func2;
func2 = func1;
*/

// 2.返回值双向协变
// 不能将返回值是联合类型的赋值给具体类型的
// 可以将返回值是具体类型的赋值给联合类型的
/*
let func1 = (x:boolean):(number | string) => x ? 18 : '张含韵';
let func2 = (x:boolean):number => 28;
// func1 = func2; 
func2 = func1;
*/ 


// 函数重载
// 不能将重载少的赋值给重载多的
// 可以将重载多的赋值给重载少
/*
function add(x:number, y:number):number;
function add(x:string, y:string):string;
function add(x, y) {
    return x + y;
}

function sub(x:number, y:number):number;
function sub(x, y) {
    return x - y;
}

// let fn = add;
// fn = sub; 

let fn = sub;
fn = add; 
*/

// 可选参数及剩余参数
// 当一个函数有剩余参数时，它被当做无限个可选参数
function func(args: any[], callback: (...args: any[]) => void) {
}

func([1, 2], (x, y, z) => console.log(x + ', ' + y + z));
func([1, 2], (x?, y?) => console.log(x + ', ' + y));
func([1, 2], (x, y?, z?) => console.log(x + ', ' + y));
```

#### 4.枚举类型知识点补充

TS中支持两种枚举, 一种是数字枚举, 一种是字符串枚举

- 数字枚举

  > 1.数字枚举的取值可以是字面量, 也可以是常量, 也可以是计算的结果
  >
  > 2.如果采用字面量对第一个成员进行赋值，下面的成员会自动递增
  >
  > 3.如果采用常量或计算结果进行赋值，则下面的成员也必须初始化

- 字符串枚举

  >1.如果采用字面量对第一个成员进行赋值，下面的成员也必须赋值
  >
  >2.采用[index]的形式不能获取到内容,需要传入[key]
  >
  >3.字符串枚举不能使用常量或者计算结果给枚举值赋值
  >
  >4.它可以使用内部的其它枚举值来赋值

- 异构枚举：枚举中既包含数字又包含字符串, 我们就称之为异构枚举

  	> 1.如果是字符串枚举, 那么无法通过原始值获取到枚举值

- 把枚举成员当做类型来使用

```typescript
export default {}

// TS中支持两种枚举, 一种是数字枚举, 一种是字符串枚举
// 1.数字枚举
// 默认情况下就是数字枚举
// 注意点: 1.数字枚举的取值可以是字面量, 也可以是常量, 也可以是计算的结果
//        2.如果采用字面量对第一个成员进行赋值，下面的成员会自动递增
//        3.如果采用常量或计算结果进行赋值，则下面的成员也必须初始化

// enum Gender{
//     Male,
//     Female
// }
// console.log(Gender.Male);
// console.log(Gender.Female);
// console.log(Gender[0]);


// const val = 100;
// const num = () => 200;
// enum Gender{
//   // Male = 1,
//   // Female

//   Male = val,
//   Female = num()
// }


// 2.字符串枚举
// 注意点: 1.如果采用字面量对第一个成员进行赋值，下面的成员也必须赋值
//        2.采用[index]的形式不能获取到内容,需要传入[key]
//        3.字符串枚举不能使用常量或者计算结果给枚举值赋值
//        4.它可以使用内部的其它枚举值来赋值
// enum Direction {
//   up = "UP",
//   down = "DOWN"
// }
// console.log(Direction.up);
// console.log(Direction.down);
// console.log(Direction[0]); // undefined
// console.log(Direction["up"]); // UP

// const val = "金晨";
// const res = () => "王鸥";
// enum User {
//   // a = val,
//   // b = res()
//    c = "HTML",
//    d = c
// }


// 3.异构枚举
// 枚举中既包含数字又包含字符串, 我们就称之为异构枚举
// 注意点: 如果是字符串枚举, 那么无法通过原始值获取到枚举值
// enum Gender{
//   Male = 1,
//   Female = '女'
// }

// console.log(Gender.Male);
// console.log(Gender.Female);
// console.log(Gender[1]);
// console.log(Gender['女']);


// 4.把枚举成员当做类型来使用
enum Gender{
  Male ,
  Female
}
interface ITestInterface {
  age: Gender // age: (Gender.Male | Gender.Female)
}
class Person implements ITestInterface{
  // age: Gender.Male
  age: Gender.Female
}

```

#### 5.枚举类型兼容性

- 数字枚举与数字兼容
- 数字枚举与数字枚举不兼容
- 字符串枚举与字符串不兼容

```typescript
export default {}


// 数字枚举与数字兼容
/*
enum Gender{
    Male,
    Female
}
let value:Gender;
value = Gender.Male;
value = 100;
 */

// 数字枚举与数字枚举不兼容
/*
enum Gender{
    Male, // 0
    Female // 1
}
enum Animal{
    Dog, // 0
    Cat // 1
}
let value:Gender;
value = Gender.Male;
value = Animal.Dog; // 报错
*/

// 字符串枚举与字符串不兼容

// enum Gender{
//     Male = '张若昀',
//     Female  = '唐艺昕'
// }
// let value:Gender;
// value = Gender.Male;
// value = Gender.Female;
// value = "娃嘻嘻"
```

#### 6.类的兼容性

类的工作方式与对象字面类型和接口类似，但有一个例外：它们同时具有静态和实例类型。当比较一个
类类型的两个对象时，只有实例的成员被比较。静态成员和构造函数不影响兼容性。

一个类中的私有成员和保护成员会影响其兼容性。当一个类的实例被检查兼容性时，如果目标类型包含
一个私有成员，那么源类型也必须包含一个源自同一类的私有成员。同样地，这也适用于有保护成员的
实例。这允许一个类与它的超类进行赋值兼容，但不允许与来自不同继承层次的类进行赋值兼容，否则
就会有相同的形状。

- public: 可多不可少
- private / protected: 不能互相赋值

```typescript
export default {};

// public
/*
class Animal {
  feet: number;
  age: number;
  constructor(name: string, numFeet: number) {}
}
class Size {
  feet: number;
  constructor(numFeet: number) {}
}

// 可多不可少
let a: Animal;
let s: Size;
s = a; // 正确
a = s; // 错误
*/


// private / protected
/*
class Animal {
  private feet: number;
  constructor(name: string, numFeet: number) {}
}
class Size {
  private feet: number;
  constructor(numFeet: number) {}
}

let a: Animal;
let s: Size;
s = a; // 错误
a = s; // 错误
*/
```

#### 7.泛型的兼容性

```typescript
export default {}


// 因为TypeScript是一个结构化的类型系统，类型参数只在作为成员类型的一部分被消耗时影响到结果类型。
/*
interface Empty<T> {}
let x: Empty<number>;
let y: Empty<string>;
x = y; // 正确，因为y符合x的结构
*/


// 在上面， x 和 y 是兼容的，因为它们的结构没有以区分的方式使用类型参数。通过给 Empty<T> 增加一
// 个成员来改变这个例子，显示了这是如何工作的
/*
interface NotEmpty<T> {
   data: T;
 }
 let x: NotEmpty<number>;
 let y: NotEmpty<string>;
 x = y; // 错误，因为x和y不兼容
 */
```



## 第十章 装饰器

介绍：装饰器是一种特殊类型的声明，它能够被附加到类，方法， 访问器，属性或参数上。用 `@`添加

​           装饰器本质上还是一个函数，在别的语言中已广泛使用，如： python, 但在TS中依旧为一个测试中的版本，若要启	   用实验性的装饰器特性，你必须在命令行或`tsconfig.json`里启用`experimentalDecorators`编译器选

​	  若要启用实验性的装饰器特性，你必须在命令行或`tsconfig.json`里启用`experimentalDecorators`编译器选项

> 添加到类上, 类装饰器
>
> 添加到方法上,方法装饰器
>
> 添加到访问器上,访问器装饰器
>
> 添加到属性上,属性装饰器
>
> 添加到参数上,参数装饰器

**装饰器工厂**：如果我们要定制一个修饰器如何应用到一个声明上，我们得写一个装饰器工厂函数。 *装饰器工厂*就是一个简		     单的函数，它返回一个表达式，以供装饰器在运行时调用

#### 1.类的装饰器

- 类装饰器就在类声明之前被声明
- 类装饰器被应用于类的构造函数，可以用来观察、修改或替换类定义
- 类装饰器不能用在声明文件中( .d.ts)，也不能用在任何外部上下文中（比如declare的类）
- 类装饰器表达式会在运行时当作函数被调用，类的构造函数作为其唯一的参数
- 如果类装饰器返回一个值，它会使用提供的构造函数来替换类的声明。

```typescript
export default {};

// 1.类装饰器
//  类装饰器就在类声明之前被声明
//  类装饰器被应用于类的构造函数，可以用来观察、修改或替换类定义
//  类装饰器不能用在声明文件中( .d.ts)，也不能用在任何外部上下文中（比如declare的类）
//  类装饰器表达式会在运行时当作函数被调用，类的构造函数作为其唯一的参数
//  如果类装饰器返回一个值，它会使用提供的构造函数来替换类的声明。

/* function testDecorator(constructor: any) {
  constructor.prototype.uname = "张予曦"
  constructor.prototype.show = ():void => {
    console.log(`我是${constructor.prototype.uname}`);
    
  }
}

@testDecorator
class Person {

}


let p = new Person();
(p as any).show();
 */


// 使用工厂函数
function testDecorator(flag: boolean) {
  if(flag) {
    return function (constructor: any) {
      constructor.prototype.uname = "张予曦";
      constructor.prototype.show = (): void => {
        console.log(`我是${constructor.prototype.uname}`);
      };
    };
  }else {
    return function(constructor: any) {}
  }
  
}

@testDecorator(true)
class Person {}

let p = new Person();
(p as any).show();
```

```typescript
export default {};

// 函数可以接收很多的参数，参数类型都是any,将他们放在一个数组中
// T 就相当于一个类，里面有构造函数
/* function testDecorator<T extends new(...args: any[]) => {}>(constructor: T) {
  // 直接对 constructor 做扩展
  return class extends constructor {
    name = "章若楠";
    show() {
      console.log(this.name);
      
    }
  }
}

@testDecorator
class Person {
  name: string;
  constructor(name: string) {
    this.name = name;
  }
}


let p = new Person("陈意涵");
console.log(p);
(p as any).show()
 */

function testDecorator() {
  return function <T extends new (...args: any[]) => {}>(constructor: T) {
    return class extends constructor {
      name = "章若楠";
      show() {
        console.log(this.name);
      }
    };
  };
}

const Person = testDecorator()(class {
  name: string;
  constructor(name: string) {
    this.name = name;
  }
})


// class Person {
//   name: string;
//   constructor(name: string) {
//     this.name = name;
//   }
// }

let p = new Person("陈意涵");
p.show();
```

#### 2.方法装饰器

- 方法装饰器写在在一个方法的声明之前
- 方法装饰器可以用来监视，修改或者替换方法定义。
- 方法装饰器表达式会在运行时当作函数被调用，传入下列3个参数：
  - 静态成员的类的构造函数，或者实例成员的类的原型
  - 成员的名称
  - 该成员的属性描述符

```typescript
export default {}

// 普通方法: target对应的就是 prototype
// 静态方法: target对应的就是 类的构造函数
function getNameDecorator(target: any, key: string, descriptor: PropertyDescriptor) {
  console.log(target);
  console.log(key);
  console.log(descriptor);
  
  // descriptor.writable = false;
  descriptor.value = function() {
    return "decorator"
  }
} 


class Test {
  name: string = "郑合惠子";
  constructor(name: string){
    this.name = name;
  }
  @getNameDecorator
  getName() {
    return this.name;
  }

  static show():void {
    console.log("Hello MethodDecorator");
    
  }
}


let t = new Test("aaa")
// t.getName = () => {
//   return "Hello 张雪迎"
// }
console.log(t.getName());
```

#### 3.访问器的装饰器

*访问器装饰器*声明在一个访问器的声明之前（紧靠着访问器声明）。 访问器装饰器应用于访问器的 *属性描述符*并且可以用来监视，修改或替换一个访问器的定义。 访问器装饰器不能用在声明文件中（.d.ts），或者任何外部上下文（比如 `declare`的类）里。

- 方法装饰器表达式会在运行时当作函数被调用，传入下列3个参数：
  - 对于静态成员来说是类的构造函数，对于实例成员是类的原型对象。
  - 成员的名字。
  - 成员的名字。
- TypeScript不允许同时装饰一个成员的get和set访问器。

```typescript
export default {}

function visitDecorator(target: any, key: string, descriptor: PropertyDescriptor) {
  descriptor.writable = false
}

class Test {
  private _name: string;
  constructor(name: string) {
    this._name = name;
  }
  get name() {
    return this._name;
  }
  @visitDecorator
  set name(newName: string) {
    this._name = newName;
  }
}

const t = new Test("周雨彤");
t.name = "钟楚曦"
console.log(t.name);
```

#### 4.属性的装饰器

- 属性装饰器写在一个属性声明之前（紧靠着属性声明）

- 属性装饰器表达式会在运行时当作函数被调用，传入下列2个参数：
  - 对于静态属性来说就是当前的类, 对于实例属性来说就是当前实例
  - 成员的名字

```typescript
export default {}

/* function nameDecorator(target: any, key: string): any {

}

class Test {
  @nameDecorator
  name = "任敏"
}

let t = new Test();
t.name = "周洁琼"
console.log(t.name);
 */

function nameDecorator(target: any, key: string): any {
  // const descriptor: PropertyDescriptor = {
  //   writable: false
  // };
  // return descriptor;

  // 修改的并不是实例上的 name， 而是原型上的 name
  target[key] = '秦岚';
}
class Test {
  @nameDecorator
  // name 是放在实例上的
  name = "任敏"
}

let t = new Test();
t.name = "周洁琼"
console.log(t.name);
console.log((t as any).__proto__.name);
```

#### 5.参数装饰器

- *参数装饰器*声明在一个参数声明之前（紧靠着参数声明)
- 参数装饰器应用于类构造函数或方法声明
- 数装饰器不能用在声明文件（.d.ts），重载或其它外部上下文（比如 `declare`的类）里
- 参数装饰器表达式会在运行时当作函数被调用，传入下列3个参数:
  - 对于静态成员来说是当前的类，对于实例成员是当前实例。
  - 参数所在的方法名称。
  - 参数在参数列表中的索引。

```typescript
export default {}


function paramDecorator(target: any, method: string, index: number) {
  console.log(target, method, index);
}

class Test {
  getInfo(name: string, @paramDecorator age: number) {
    console.log(name, age);
  }
}

const t = new Test();
t.getInfo('安悦溪', 18);
```

#### 6.小案例

- 需求: 利用装饰器来避免多次书写 try{} catch(e) {}

```typescript
export default {};

/* const userInfo: any = undefined;

class Test {
  getName() {
    try {
      return userInfo.name;
    } catch (e) {
      console.log(e);
    }
  }
  getAge() {
    try {
      return userInfo.age;
    } catch (e) {
      console.log(e);
    }
  }
}

const t = new Test();
t.getName();
t.getAge(); */

/* const userInfo: any = undefined;

function catchError(target: any, key: string, descriptor: PropertyDescriptor) {
  const fn = descriptor.value;
  descriptor.value = function () {
    try {
      fn();
    } catch (e) {
      console.log("userInfo上该属性不存在");
    }
  };
}

class Test {
  @catchError
  getName() {
    return userInfo.name;
  }
  @catchError
  getAge() {
    return userInfo.age;
  }
}

const t = new Test();
t.getName();
t.getAge(); */


const userInfo: any = undefined;
function catchError(msg: string) {
  return function(target: any, key: string, descriptor: PropertyDescriptor) {
    const fn = descriptor.value;
    descriptor.value = function() {
      try {
        fn();
      } catch (e) {
        console.log(msg);
      }
    };
  };
}

class Test {
  @catchError('userInfo.name 不存在')
  getName() {
    return userInfo.name;
  }
  @catchError('userInfo.age 不存在')
  getAge() {
    return userInfo.age;
  }
}

const t = new Test();
t.getName();
t.getAge();
```



## 第十一章 混入Mixins

**介绍**:  除了传统的面向对象继承方式，还流行一种通过可重用组件创建类的方式，就是联合另一个简单类的代码。 你可能在Scala等语言里对mixins及其特性已经很熟悉了，但它在JavaScript中也是很流行的。

**作用**:  解决TS中继承一次只能继承一个类的问题

**注意点:** 类的混入不能混入属性名

```typescript
// 对象混入

/* let nameObj = { name:'王楚然' };
let ageObj = { age:18 };
// 需求: 想要让nameObj拥有age属性
Object.assign(nameObj, ageObj);
console.log(nameObj);
console.log(ageObj); */


// 类混入
class Name {
  name: string = "毛晓彤";
  getName():void {
    console.log("我是毛晓彤");
  }
}

class Age {
  age: number = 18;
  getAge(): void {
    console.log("我今年18岁");
  }
}

// class Person extends Name, Age {}

class Person implements Name, Age {
  age: number;
  name: string;
  getAge: () => void;
  getName: () => void;
}

function Mixins(target: any, from: any[]) {
  // console.log(target);
  // console.log(from);
  from.forEach(item => {
    // console.log(item);
    Object.getOwnPropertyNames(item.prototype).forEach(name => {
      target.prototype[name] = item.prototype[name];
    })
  })
}
Mixins(Person, [Name, Age])

let p = new Person();
p.getAge();
p.getName();
// console.log(p.name);
// console.log(p.age);
```

## 第十二章 模块与命名空间

#### 1.TS中的模块

TypeScript 模块的设计理念是可以更换的组织代码。

两个模块之间的关系是通过在文件级别上使用 import 和 export 建立的

模块使用模块加载器去导入其它的模块。 在运行时，模块加载器的作用是在执行此模块代码前去查找并执行这个模块的所有依赖。 大家最熟知的JavaScript模块加载器是服务于 Node.js 的 CommonJS 和服务于 Web 应用的 Require.js。

模块导出使用关键字 **export** 关键字，语法格式如下：

> // 文件名 : SomeInterface.ts 
> export interface SomeInterface { 
> // 代码部分
> }

要在另外一个文件使用该模块就需要使用 **import** 关键字来导入:

> import someInterfaceRef = require("./SomeInterface");

```typescript
// moduleTest.ts中的代码
export interface Iperson {
  name: string;
  age: number;
  sex: string;
  show(): void;
}

export const obj = {
  name: "陈都灵"
}
```

```typescript

// JS中的模块
/*
1.默认导入与导出
// 注意点: 这里导入和导出的名字，可以不一致
export default xxx
import ooo from "路径"
*/

/* 
1.按需导入与导出
注意点: 这里导入和导出的名字必须一致
export xxx;
import {xxx} from "路径"
*/

// node中的模块
/* 
1.exports.xxx = xxx
const xxx = require("path");
const {xx, xx} = require("path");

2.module.exports.xxx = xxx
const xxx = require("path");
const {xx, xx} = require("path");
*/

// 3.TS中的模块
// 默认在JS中是不兼容上面两种混合使用的，而JS中兼容混合写法
import Test = require("./moduleTest")

export class UserInfo implements Test.Iperson { 
  name = "高圆圆";
  age = 18;
  sex = "女";
  show() {
    console.log("你好");
    
  }
}

let u = new UserInfo();
console.log(u.name);


import { obj } from "./moduleTest";
console.log(obj);
```

#### 2.TS中的命名空间

项目开发过程中，我们会发现我们的命名是有严格规范的，我们不能随意的去起名字，但若是都采用尽量标准化的方式去命名，我们又无法避免的会造成污染，TypeScript提供了namespace 避免这个问题出现

- 在TS1.5之前被叫做内部模块，主要用于组织代码，避免命名冲突
- 本质就是定义一个大对象, 把变量/方法/类/接口...的都放里面
- 通过 `export` 导出
- 通过 `namespace` 定义

```typescript
namespace A {
  export const a = 100;
}

console.log(A.a);

// 嵌套命名空间
namespace B {
  export const b = 200;
  export namespace C {
    // export const b = 300;
    export const c = 300;
  }
}

console.log(B.b);
// console.log(B.C.b);
console.log(B.C.c);

// 简化命名空间
import c = B.C.c
console.log(c);


// namespaceTest.ts内容
export namespace D {
    export const d = 1000;
}    
// 主文件
// 从其他文件引入命名空间
import { D } from "./namespaceTest";
console.log(D.d);    
```

#### 3.三斜杠语法

三斜线指令是包含单个[XML](https://so.csdn.net/so/search?q=XML&spm=1001.2101.3001.7020)标签的单行注释。 注释的内容会做为编译器指令使用。

如果一个命名空间在一个单独的 TypeScript 文件中，则最应使用三斜杠 /// 引用它，语法格式如下：

`/// <reference path = "xxx.ts" />`

```typescript
// namespaceTest2.ts
namespace  User{ 
  export interface IName { 
      uname: string;
  }
}
    
namespace User {
  export namespace UserInfo {
    export interface IName {
      uname: string;
    }
  }
}    
    
    
// index.ts
/// <reference path = "./namespaceTest2.ts" /> 


const a: User.IName = {
  uname:  "万茜"
}


const a: User.UserInfo.IName = {
  uname:  "万茜"
}

console.log(a);

```

#### 4.声明合并

- 接口的合并

  注意点: 1.如果名字一样会进行合并

  ​             2.如果里面出现了同名函数，则会转化为函数重载

- 命名空间合并

  注意点: 1.与接口一样,若名称相同则会进行合并

  ​             2.同名的命名空间中不能出现同名的变量,方法等

  ​             3.命名空间还可以和同名的类/函数/枚举合并:

  ​			命名空间与类合并:    1.say会被放在 prototype上   2.类必须定义在命名空间的前面 

  ​			命名空间和函数合并: 函数必须定义在命名空间的前面

  ​		        命名空间和枚举合并：没有先后顺序的要求

```typescript
export default {};

// 1.接口

// interface ITestInterface {
//     name:string;
// }
// interface ITestInterface {
//     age:number;
// }

// class Person implements ITestInterface{
//     name:string = "文咏珊";
//     age:number = 18;

// }

interface ITestInterface {
  show(value: number): number;
}
interface ITestInterface {
  show(value: string): number;
}

const func: ITestInterface = {
  show(value: any): number {
    if (typeof value === "string") {
      return value.length;
    } else {
      return value.toFixed();
    }
  },
};
console.log(func.show("世界上最遥远的距离就是,你是if而我是else, 似乎一直相伴但又永远相离"));
console.log(func.show("世界上最痴心的等待,是我当case而你当switch,或许永远都选不上自己"));
console.log(func.show("世界上最真情的相依,是你在try我在catch。无论你发神马脾气,我都默默承受,静静处理。到那时,再来期待我们的finally"));
console.log(func.show(3.14));

// 命名空间与类合并
// 注意点: 1.say会被放在 prototype上
//        2.命名空间只能放在与之合并的类之后 
/*
class Person {
    say():void{
        console.log("say 孙怡");
    }
}
namespace Person{
    export const hi = ():void=>{
        console.log('hi 孙怡');
    }
}
console.dir(Person);
*/


// 命名空间和函数合并
// 注意点: 函数必须定义在命名空间的前面
/*
function getCounter() {
    getCounter.count++;
    console.log(getCounter.count);
}
namespace getCounter{
    export let count:number = 0;
}
getCounter()
*/

// 命名空间和枚举合并
// 注意点: 没有先后顺序的要求
enum Gender {
    Male,
    Female
}
namespace Gender{
    export const Yao:number = 666;
}
console.log(Gender);
```



## 第十三章 配置类相关内容

#### 1.ts.tsconfig.json文件

```typescript
{
  "compilerOptions": {
    /* Visit https://aka.ms/tsconfig to read more about this file */

    /* Projects */
    // TS编译器在第一次编译之后会生成一个存储编译信息的文件，第二次编译会在第一次的基础上进行增量编译，可以提高编译的速度
    // "incremental": true,

    // 启用允许TypeScript项目与项目引用一起使用的约束
    // "composite": true,

    // 增量编译文件的存储位置
    // "tsBuildInfoFile": "./.tsbuildinfo",

    // 在引用组合项目时，禁用首选源文件而不是声明文件。
    // "disableSourceOfProjectReferenceRedirect": true,

    // 编辑时从多项目引用检查中选择一个项目。
    // "disableSolutionSearching": true,

    // 减少TypeScript自动加载的项目数。
    // "disableReferencedProjectLoad": true,

    /* Language and Environment */
    "target": "es2016",
    // TS需要引用的库，即声明文件，es5 默认引用dom、es5、scripthost,如需要使用es的高级版本特性，
    //通常都需要配置，如es8的数组新特性需要引入"ES2019.Array",
    // "lib": [],

    // 指定生成的JSX代码。
    "jsx": "preserve",

    // 启用对TC39第2阶段草稿装饰器的实验支持。
    // "experimentalDecorators": true,

    // 为源文件中的修饰声明发出设计类型元数据。
    // "emitDecoratorMetadata": true,

    // 指定针对React JSX emit时使用的JSX工厂函数，例如“React”。createElement”或“h”。
    // "jsxFactory": "",

    // 指定针对React JSX emit（例如“React”）时用于片段的JSX片段引用。“片段”或“片段”。
    // "jsxFragmentFactory": "",

    // 指定使用“JSX:react JSX*”时用于导入JSX工厂函数的模块说明符。
    // "jsxImportSource": "",

    // 指定为“createElement”调用的对象。这仅适用于针对“react”JSX emit的情况。
    // "reactNamespace": "",

    // 禁用包含任何库文件，包括默认库。d、 ts。
    // "noLib": true,

    // 发出符合ECMAScript标准的类字段。
    // "useDefineForClassFields": true,

    // 控制用于检测模块格式JS文件的方法。
    // "moduleDetection": "auto",

    /* Modules */
    // 生成代码的模板标准,可选es6模式 amd  umd 等
    "module": "commonjs",

    // 指定输出文件目录(用于输出)，用于控制输出目录结构
    // "rootDir": "./",

    // 模块解析策略，ts默认用node的解析策略，即相对的方式导入
    // "moduleResolution": "node",

    // 解析非相对模块的基地址，默认是当前目录
    // "baseUrl": "./",

    // 路径映射，相对于baseUrl
    // "paths": {},

    // 将多个目录放在一个虚拟目录下，用于运行时，即编译后引入文件的位置可能发生变化，这也设置可以虚拟src和out在同一个目录下，不用再去改变路径也不会报错
    // "rootDirs": [],

    // 声明文件目录，默认时node_modules/@types
    // "typeRoots": [],

    // 加载的声明文件包
    // "types": [],

    // 允许在模块中全局变量的方式访问umd模块
    // "allowUmdGlobalAccess": true,

    // 解析模块时要搜索的文件名后缀列表。
    // "moduleSuffixes": [],

    // 启用导入。json文件。
    // "resolveJsonModule": true,

    // 不允许“import”、“require”或“reference”扩展TypeScript应添加到项目中的文件数。
    // "noResolve": true,

    /* JavaScript Support */
    // 允许编译器编译JS，JSX文件
    // "allowJs": true,

    // 允许在JS文件中报错，通常与allowJS一起使用
    // "checkJs": true,

    // 指定用于检查“node\u modules”中的JavaScript文件的最大文件夹深度。仅适用于“allowJs”。
    // "maxNodeModuleJsDepth": 1,

    /* Emit */
    // 生成声明文件，开启后会自动生成声明文件
    // "declaration": true,

    // 为声明文件生成sourceMap
    // "declarationMap": true,

    // 只生成声明文件，而不会生成js文件
    // "emitDeclarationOnly": true,

    // 生成目标文件的sourceMap文件
    // "sourceMap": true,

    // 将多个相互依赖的文件生成一个文件，可以用在AMD模块中，即开启时应设置"module": "AMD",
    // "outFile": "./",

    // 指定输出目录
    // "outDir": "./",

    // 删除注释
    // "removeComments": true,

    // 不输出文件,即编译后不会生成任何js文件
    // "noEmit": true,

    // 通过tslib引入helper函数，文件必须是模块
    // "importHelpers": true,

    // 为仅用于类型的导入指定发射/检查行为。
    // "importsNotUsedAsValues": "remove",

    // 为迭代发出更符合要求、但冗长且性能较差的JavaScript。
    // "downlevelIteration": true,

    // 指定调试器查找参考源代码的根路径。
    // "sourceRoot": "",

    // 指定调试器应该定位映射文件的位置，而不是生成的位置。
    // "mapRoot": "",

    // 生成目标文件的inline SourceMap，inline SourceMap会包含在生成的js文件中
    // "inlineSourceMap": true,

    // 在发出的JavaScript内的sourcemaps中包含源代码。
    // "inlineSources": true,

    // 在输出文件的开头发出UTF-8字节顺序标记（BOM）。
    // "emitBOM": true,

    // 设置用于发射文件的换行符。
    // "newLine": "crlf",

    // 禁用在JSDoc注释中包含“@internal”的声明。
    // "stripInternal": true,

    // Disable generating custom helper functions like '__extends' in compiled output. 
    // "noEmitHelpers": true,

    // 如果报告了任何类型检查错误，则禁用发送文件。
    // "noEmitOnError": true,

    // 禁用删除生成代码中的“const enum”声明。
    // "preserveConstEnums": true,

    // 指定生成声明文件存放目录
    // "declarationDir": "./",

    // 在JavaScript输出中保留未使用的导入值，否则会被删除。
    // "preserveValueImports": true,

    /* Interop Constraints */
    // "isolatedModules": true,
    // "allowSyntheticDefaultImports": true,
    "esModuleInterop": true,
    // "preserveSymlinks": true,
    "forceConsistentCasingInFileNames": true,

    /* Type Checking */
    "strict": true,

    // 确保每个文件都可以安全传输，而不依赖于其他导入。
    // "noImplicitAny": true,

    // 不允许把null、undefined赋值给其他类型的变量
    // "strictNullChecks": true,

    // 不允许函数参数双向协变
    // "strictFunctionTypes": true,

    // 严格的bind/call/apply检查
    // "strictBindCallApply": true,

    // 类的实例属性必须初始化
    // "strictPropertyInitialization": true,

    // 不允许this有隐式的any类型
    // "noImplicitThis": true,

    // 默认catch子句变量为“unknown”而不是“any”。
    // "useUnknownInCatchVariables": true,

    // 在代码中注入'use strict
    // "alwaysStrict": true,

    // 检查只声明、未使用的局部变量(只提示不报错)
    // "noUnusedLocals": true,

    // 检查未使用的函数参数(只提示不报错)
    // "noUnusedParameters": true,

    // 将可选属性类型解释为编写的，而不是添加“undefined”。
    // "exactOptionalPropertyTypes": true,

    //  每个分支都会有返回值
    // "noImplicitReturns": true,

    // 防止switch语句贯穿(即如果没有break语句后面不会执行)
    // "noFallthroughCasesInSwitch": true,

    // 使用索引访问时，将“undefined”添加到类型。
    // "noUncheckedIndexedAccess": true,

    // 对使用索引类型声明的键强制使用索引访问器。
    // "noPropertyAccessFromIndexSignature": true,

    // 禁用未使用标签的错误报告。
    // "allowUnusedLabels": true,

    // 禁用无法访问代码的错误报告
    // "allowUnreachableCode": true,

    /* Completeness */
    // 跳过类型检查。d、 TypeScript中包含的ts文件。
    // "skipDefaultLibCheck": true,

    // 跳过类型检查全部。d、 ts文件。
    "skipLibCheck": true
  }
}

```

#### 2.使用rollup打包TS文件

安装依赖：

- 全局安装rollup **npm install rollup -g**
- 安装TypeScript   **npm install typescript -D**
- 安装TypeScript 转换器 **npm install rollup-plugin-typescript2 -D**
- 安装代码压缩插件 **npm install rollup-plugin-terser -D**
- 安装rollupweb服务 **npm install rollup-plugin-serve -D**
- 安装热更新 **npm install rollup-plugin-livereload -D**
- 安装配置环境变量用来区分本地和生产  **npm install cross-env -D**

步骤：

​	1.安装依赖

​	2.npm init -y 创建配置package.json文件

​        3.创建 src    public   rollup.config.js文件

​        4.配置  rollup.config.js

package.json文件：

```json
{
  "name": "rollupTs",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "cross-env NODE_ENV=production rollup -c",
    "dev": "cross-env NODE_ENV=development rollup -c"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "cross-env": "^7.0.3",
    "rollup-plugin-livereload": "^2.0.5",
    "rollup-plugin-serve": "^1.1.0",
    "rollup-plugin-terser": "^7.0.2",
    "rollup-plugin-typescript2": "^0.31.1",
    "typescript": "^4.5.5"
  }
}
```

rollup.config.js文件

```js
import path from "path"
import livereload from "rollup-plugin-livereload"
import serve from "rollup-plugin-serve"
import { terser } from "rollup-plugin-terser"
import ts from "rollup-plugin-typescript2"

export default {
  input: "./src/index.ts", 

  output: {
    file: path.resolve(__dirname, "./dist/index.js"),
    sourcemap: true,
    format: "umd", 
  },

  plugins: [
    ts(),
    livereload(),
    terser(),
    serve({
      open: true,
      port: 8080,
      openPage: "/public/index.html"
    }),
    
  ]
}

console.log(process.env);
```

#### 3.使用webpack打包TS文件

安装依赖：

- 安装webpack环境 **npm i webpack webpack-cli webpack-dev-server -D**
- 安装TypeScript   **npm install typescript -D**
- 编译TS **npm install ts-loader -D**
- 热更新服务 **npm install  webpack-dev-server -D**
- HTML模板 **npm install html-webpack-plugin -D**

步骤：

​	1.安装依赖

​	2.npm init -y 创建配置package.json文件

​	3.创建初始文件夹

​	4.进行配置

package.json文件

```json
{
  "name": "webpackProject",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack",
    "dev": "webpack serve --open"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "html-webpack-plugin": "^5.5.0",
    "ts-loader": "^9.3.1",
    "typescript": "^4.7.4",
    "webpack": "^5.73.0",
    "webpack-cli": "^4.10.0",
    "webpack-dev-server": "^4.9.3"
  }
}
```

webpack.config.js文件

```js
const path = require("path")
const htmlWebpackPlugin = require("html-webpack-plugin")

module.exports = {
  entry: "./src/index.ts",

  output: {
    path: path.resolve(__dirname, "dist"),
    filename: "index.js"
  },

  module: {
    rules: [
      {test: /\.ts$/, use: 'ts-loader', exclude:/node_modules/}
    ]
  },
  mode: "development",

  resolve: {
    extensions: [".ts", ".js"]
  },

  plugins: [
    new htmlWebpackPlugin({
      template: "./public/index.html"
    })
  ]
}
```

#### 4.TS描述文件声明

TypeScript 作为 JavaScript 的超集，在开发过程中不可避免要引用其他第三方的 JavaScript 的库。虽然通过直接引用可以调用库的类和方法，但是却无法使用TypeScript 诸如类型检查等特性功能。为了解决这个问题，需要将这些库里的函数和方法体去掉后只保留导出类型声明，而产生了一个描述 JavaScript 库和模块信息的声明文件。通过引用这个声明文件，就可以借用 TypeScript 的各种特性来使用库文件了。

假如我们想使用第三方库，比如 jQuery等等

声明文件以 **.d.ts** 为后缀 如：`hello.d.ts`

声明文件或模块的语法格式如下：`declare module Module_Name { }`

**很多流行的第三方库的声明文件不需要我们定义了**

- https://github.com/DefinitelyTyped/DefinitelyTyped/tree/master/types
- https://www.typescriptlang.org/dt/search?search=

#### 5.安装vue3_ts并与vue3_js对比

- 创建一个vue_js项目   `vue create 项目名`
- 创建一个vue_ts项目   `vue create 项目名`
- 创建一个vue_vite_ts项目  `npm init vite@latest`

目录对比：

​                              	![1657953628208](C:\Users\11\AppData\Roaming\Typora\typora-user-images\1657953628208.png)

![1657953714476](C:\Users\11\AppData\Roaming\Typora\typora-user-images\1657953714476.png)

![1657953815040](C:\Users\11\AppData\Roaming\Typora\typora-user-images\1657953815040.png)

#### 6.安装react_ts环境并与react_js对比

- 安装一个react_js项目 `npx create-react-app 项目名`
- 安装一个react_ts项目 `npx create-react-app 项目名 --template typescript`
- 安装一个react_vite_ts项目 npm init vite@latest

目录对比

![1657955046363](C:\Users\11\AppData\Roaming\Typora\typora-user-images\1657955046363.png

​                                        ![1657955074568](C:\Users\11\AppData\Roaming\Typora\typora-user-images\1657955074568.png)   

![1657955117658](C:\Users\11\AppData\Roaming\Typora\typora-user-images\1657955117658.png)

![1657955159220](C:\Users\11\AppData\Roaming\Typora\typora-user-images\1657955159220.png)











​	  

