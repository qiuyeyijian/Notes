## Ajax

### 1. 什么是Ajax

AJAX = Asynchronous JavaScript and XML（异步的 JavaScript 和 XML）。

AJAX 不是新的编程语言，而是一种使用现有标准的新方法。

AJAX 是与服务器交换数据并更新部分网页的艺术，在不重新加载整个页面的情况下。

### 2. 使用Ajax的五步

> 1. 创建一个异步对象
> 2. 设置请求方式和请求地址
> 3. 发送请求
> 4. 监听状态的变化
> 5. 处理返回的结果

```javascript
 function objToString(data) {
    //加上时间参数，使得每次访问地址不一样
    data.time = new Date().getTime();
    let res = [];

    for (let key in data) {
        if (data.hasOwnProperty(key)) {
            res.push(encodeURIComponent(key) + "=" + encodeURIComponent(data[key]));
        }
    }
    return res.join("&");
}

function ajax(option) {
    //0. 拿到参数
    let str = objToString(option.data);
    //1. 创建一个异步对象
    let xmlHttp, timer;
    if (window.XMLHttpRequest) {
        //code for IE7+, Firefox, Chrome, Opera, Safari
        xmlHttp = new XMLHttpRequest();
    } else {
        //Code for IE6, IE5
        xmlHttp = new ActiveXObject("Microsoft.XMLHTTP");
    }
    //2. 设置请求方式和请求地址
    if (option.type.toLowerCase() === "get") {
        xmlHttp.open(option.type, option.url + "?" + str, true);
        //3. 发送请求
        xmlHttp.send();
    }else {
        //2. 设置请求方式和请求地址
        xmlHttp.open(option.type, option.url, true);
        //setRequestHeader只能写在这两个中间
        xmlHttp.setRequestHeader("content-type","application/x-www-form-urlencoded");
        //3. 发送请求
        xmlHttp.send(str);
    }

    //4. 监听对象状态变化
    xmlHttp.onreadystatechange = function (ev2) {
        if (xmlHttp.readyState === 4) {
            //请求成功，清除定时器
            clearInterval(timer);
            if (xmlHttp.status >= 200 && xmlHttp.status <= 300 || xmlHttp.status === 304) {
                // console.log("接收到服务器返回的数据");
                option.success(xmlHttp);
            }
            else {
                option.error(xmlHttp);
            }

        }
    };
    //设置一个定时器，处理响应时间，超过就断开
    if (option.timeout) {
        timer = setInterval(function () {
            alert("中断请求");
            xmlHttp.abort();
            clearInterval(timer);
        }, option.timeout)
    }
}
```

上面是自己封装的ajax代码，只是练习使用，jQuery中封装的更完善。

其中这段代码是为了兼容ie浏览器（ie浏览器缓存，同一URL只会返回同一数据），后面拼接一个时间创建的字符串，可以使得每次请求地址不一样，从而每次都可以获得最新的数据

```javascript
 xmlHttp.open("GET", url + "?t=" + (new Date().getTime()), true);
```





### 3. xml

php执行结果有中文，必须在php文件顶部设置

```php
header("content-type:text/html;charset=utf-8");
```

如果php中需要返回xml数据，也必须在php文件顶部设置

```php
header("content-type:text/xml;charset=utf-8");
```



### 4. cookie

cookie： 会话跟踪技术	客户端

session： 会话跟踪技术	服务端



cookie的作用：

将网页数据保存到浏览器中



cookie的生命周期：

默认情况下生命周期是一次会话（浏览器被关闭）

如果通过`expires`设置了过期时间，并且过期时间没有过期，那么下次打开还是存在

如果已经过期了，那么会立即删除保存的数据



cookie注意点：

cookie默认不会保留任何数据

cookie 不能一次设置多条数据，只能一条一条地设置

cookie有大小和数量的限制

个数：20-50个

大小：4kb左右





cookie的作用范围

同一个浏览器，同一个路径下

如果在统一浏览器中，默认情况下，下一级路径可以访问

如果需要在上一级路径也能访问，则需要添加`path=/`,将cookie保存到根路径下

```js
document.cookie = "path=/"
```



如果需要在同一个根域名，不同的子域名访问则需要添加`domain=qiuyeyijian.com`，则无论是在`edu.qiuyeyijianl.com`还是在`cdn.qiuyeyijian.com`下都能访问；



