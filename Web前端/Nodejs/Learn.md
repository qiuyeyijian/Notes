## learn

### 1. 创建web服务器

``` js
var http = require('http');

http.createServer(function (request, response) {

    // 发送 HTTP 头部 
    // HTTP 状态值: 200 : OK
    // 内容类型: text/plain
    response.writeHead(200, {'Content-Type': 'text/plain'});

    // 发送响应数据 "Hello World"
    response.end('Hello World\n');
}).listen(8888);

// 终端打印如下信息
console.log('Server running at http://127.0.0.1:8888/');
```

### 2. 回调函数

``` js
//阻塞I/O实例
var fs = require("fs");

var data = fs.readFileSync('input.txt');

console.log(data.toString());
console.log("程序执行结束!");

//非阻塞I/O实例
var fs = require("fs");

fs.readFile('input.txt', function (err, data) {
    if (err) return console.error(err);
    console.log(data.toString());
});

console.log("程序执行结束!");
```



### 单元测试

单元测试是指对软件中的最小可测试单元进行检查和验证，一次又称为模块测试。在Node.js中，单元测试往往是针对某个函数或者API进行正确性验证，以保证代码的可用性。

这在团队开发中显得尤其重要，一位团队成员并不了解其他成员代码的写法以及可用性。通过单元测试，既保证了单个成员所写代码的可用性，又可以让其他成员通过测试内容了解其函数或者API的使用。

单元测试有许多风格，常见的风格有**行为驱动开发（BDD）**和**测试驱动开发（TDD）**

> **行为驱动开发（BDD）**是一种敏捷软件开发的技术，鼓励软件项目中的开发者、QA和非技术人员或者商业参与者之间的协作。也就是说，行为驱动开发关注的是整个系统最终实现是否与用户期望一致。
>
> **测试驱动开发（TDD）**是一种软件开发过程中的应用方法，最早在极限编程中倡导，基本思想是先写测试程序，然后编码实现功能。测试驱动开发的目的是取得快速反馈，使所有功能都是可用的。