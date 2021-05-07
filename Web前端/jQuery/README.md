## jQuery 学习笔记

### 1.

jQuery 入口函数:

```
$(document).ready(function(){
    // 执行代码
});
或者
$(function(){
    // 执行代码
});                                                                                   
```

JavaScript 入口函数:

```
window.onload = function () {
    // 执行代码
}
```

jQuery 入口函数与 JavaScript 入口函数的区别：

-  jQuery 的入口函数是在 html 所有标签(DOM)都加载之后，就会去执行。
-  JavaScript 的 window.onload 事件是等到所有内容，包括外部图片之类的文件加载完后，才会执行。



![img](%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/20171231003829544.jpeg)



### 2. ":"和 "[]"的理解

关于 **:** 和 **[]** 这两个符号的理解

**：**可以理解为种类的意思，如：**p:first**，**p** 的种类为第一个。

**[]** 很自然的可以理解为属性的意思，如：**[href]** 选取带有 **href** 属性的元素。





### 3. 事件

页面对不同访问者的响应叫做事件。

事件处理程序指的是当 HTML 中发生某些事件时所调用的方法。

实例：

- 在元素上移动鼠标。
- 选取单选按钮
- 点击元素

在事件中经常使用术语"触发"（或"激发"）例如： "当您按下按键时触发 keypress 事件"。

常见 DOM 事件：

| 鼠标事件                                                     | 键盘事件                                                     | 表单事件                                                  | 文档/窗口事件                                             |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :-------------------------------------------------------- | :-------------------------------------------------------- |
| [click](https://www.runoob.com/jquery/event-click.html)      | [keypress](https://www.runoob.com/jquery/event-keypress.html) | [submit](https://www.runoob.com/jquery/event-submit.html) | [load](https://www.runoob.com/jquery/event-load.html)     |
| [dblclick](https://www.runoob.com/jquery/event-dblclick.html) | [keydown](https://www.runoob.com/jquery/event-keydown.html)  | [change](https://www.runoob.com/jquery/event-change.html) | [resize](https://www.runoob.com/jquery/event-resize.html) |
| [mouseenter](https://www.runoob.com/jquery/event-mouseenter.html) | [keyup](https://www.runoob.com/jquery/event-keyup.html)      | [focus](https://www.runoob.com/jquery/event-focus.html)   | [scroll](https://www.runoob.com/jquery/event-scroll.html) |
| [mouseleave](https://www.runoob.com/jquery/event-mouseleave.html) |                                                              | [blur](https://www.runoob.com/jquery/event-blur.html)     | [unload](https://www.runoob.com/jquery/event-unload.html) |
| [hover](https://www.runoob.com/jquery/event-hover.html)      |                                                              |                                                           |                                                           |

想要自定义事件，必须满足两个条件

1. 事件必须通过on绑定

2. 事件必须通过trigger来触发

   ```javascript
   $("btn").on("myClick", function(){
       alert("hello");
   });
   $("btn").trigger("myClick");
   ```

   

#### 4. **attr** 和 **prop** 的区别介绍：

对于 HTML 元素本身就带有的固有属性，在处理时，使用 **prop** 方法。

对于 HTML 元素我们自己自定义的 DOM 属性，在处理时，使用 **attr** 方法。

实例 1：

```
<a href="https://www.runoob.com" target="_self" class="btn">菜鸟教程</a>
```

这个例子里 **** 元素的 DOM 属性有: **href、target** 和 **class**，这些属性就是 **** 元素本身就带有的属性，也是 W3C 标准里就包含有这几个属性，或者说在 IDE 里能够智能提示出的属性，这些就叫做固有属性。处理这些属性时，建议使用 **prop** 方法。

```
<a href="#" id="link1" action="delete" rel="nofollow">删除</a>
```

这个例子里 **** 元素的 DOM 属性有: **href、id** 和 **action**，很明显，前两个是固有属性，而后面一个 **action** 属性是我们自己自定义上去的，**** 元素本身是没有这个属性的。这种就是自定义的 DOM 属性。处理这些属性时，建议使用 **attr** 方法。



### 5. GET 和 POST请求

1. 可以通过form标签的method属性指定发送请求的类型
2. 如果get请求会将提交的数据拼接到URL后面
3. 如果是post请求，会将提交的数据放到请求头中

相同点：

都是将数据提交到远程服务器

不同点：

|                  | GET                    | POST                 |
| ---------------- | ---------------------- | -------------------- |
| 提交数据存储位置 | 将数据放到URL后面      | 将数据放到请求头中   |
| 提交数据大小限制 | 有大小限制             | 没有大小限制         |
| 应用场景         | 提交非敏感数据和小数据 | 提交敏感数据和大数据 |



### 6. ajax



#### post

```js
$.ajax({
     //请求类型，这里为POST
     type: 'POST',
     //你要请求的api的URL
     url: url ,
     //是否使用缓存
     cache:false,
     //数据类型，这里我用的是json
     dataType: "json", 
     //必要的时候需要用JSON.stringify() 将JSON对象转换成字符串
     data: JSON.strigify({key:value}), //data: {key:value}, 
     //添加额外的请求头
     headers : {'Access-Control-Allow-Origin':'*'},
     //请求成功的回调函数
     success: function(data){
        //函数参数 "data" 为请求成功服务端返回的数据
},
});
```



#### get

```javascript
$.ajax({
  url: url,
  data: data,
  success: success,
  dataType: dataType
});
```



#### php https

```php


   //模拟get
function curl_get_https($url)
{
    $curl = curl_init();
    curl_setopt($curl, CURLOPT_URL, $url);
    curl_setopt($curl, CURLOPT_HEADER, 0);
    curl_setopt($curl, CURLOPT_RETURNTRANSFER, 1);
    curl_setopt($curl, CURLOPT_SSL_VERIFYPEER, false);  // 跳过检查
    curl_setopt($curl, CURLOPT_SSL_VERIFYHOST, false);  // 跳过检查
    $tmpInfo = curl_exec($curl);
    curl_close($curl);
    return $tmpInfo;   //返回json对象
}
```



### 解析Json 字符串



```js
let json = $.parseJSON(jsonString);
console.log(json.key1);
```



### 设置和去除 readonly 和 disable属性

**两种方法设置disabled属性**

```js
　　$('#areaSelect').attr("disabled",true);
　　$('#areaSelect').attr("disabled","disabled");
```

　　**三种方法移除disabled属性**

```js
　　$('#areaSelect').attr("disabled",false);
　　$('#areaSelect').removeAttr("disabled");
　　$('#areaSelect').attr("disabled","");
```

