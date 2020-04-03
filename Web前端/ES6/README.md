## learn

### 1. 字符串相关扩展

``` 
includes(参数一， 参数二)    //判断字符串是否包含指定的字串，有的话返回true, 否则返回false。
                           //参数一：匹配的字符串，参数二：从第几个开始匹配

startsWith()               //判断字符串是否以特定的字串开始匹配
endsWith()                 //判断字符串是否以特定的字串结束
```

``` 
console.log('hello world'.includes('word', 7));

let url = 'admin/index.php';
console.log(url.startsWith('admin'))
cosole.log(url.endsWith('php'));
```

``` 
//模板字符串
//反引号表示模板，模板中可以有格式，通过${}方式填充数据
let tpl = `
    <div>
         <span>${obj.username}</span>
         <span>${obj.age}</span>
         <span>${obj.gender}</span> 
    </div>

`;

console.log(tpl);

```

### 2. 函数相关扩展

> 1. 参数默认值
> 2. 参数结构赋值
> 3. rest 参数
> 4. ... 扩展运算符

``` 
//参数默认值

function foo (param = 'nihao') {
    console.log(param);
}

foo();
foo('hello, kitty');

//参数结构赋值
function foo ({username = 'lisi', age = 12}= null) {
    console.log(username, age);
}

foo({username:'zhang', age: '19'});

//rest 参数
function foo (a, b, ...param) {
    console.log(a);
    console.log(b);
    console.log(param);
}

foo(1, 2, 3, 4, 5);

//... 扩展运算符

function foo (a, b, c, d, e, f, g) {
    console.log(a + b + c + d + e + f +g);
}

let arr = [1, 2, 3, 4, 5, 6, 7];

foo(...arr);


//应用, 数组合并
let arr1 = [1, 2, 3];
let arr2 = [4, 5, 6];
let arr3 = [...arr1, ...arr2];

console.log(arr3);

```

### 3. 箭头函数

``` 
function foo () {
    console.log('hello');
}
foo();
//等价于
let foo = () => console.log('hello');
foo();

//多个参数必须用小括号包住
let foo = (a,b) => {
    let c = 1;
    console.log(a + b + c);
}
foo(1, 2);

//作为匿名函数传递参数
let arr = [1, 2, 3];

arr.forEach(function(element, index) {
console.log(element, index);
});
//相当于
arr.forEach((element, index) => {
console.log(element, index);
}); 

```

> 箭头函数注意事项;
>
> 1. 箭头函数中的this取决于函数的定义，而不是调用
>
> 2.  箭头函数不可以new
>
> 3.  箭头函数不可以使用arguments获取参数列表， 可以使用rest 参数代替
>
>    ``` 
>    let foo (...param) => {
>    console.log(param);
>    };
>    foo(1, 3, 4);
>    ```
>
>    