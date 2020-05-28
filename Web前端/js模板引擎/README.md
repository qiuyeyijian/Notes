## 快速上手超级实用的JS模板引擎--doT.js



### 总体结构

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>dot</title>
    
    <!--引入jQuery和doT.js文件 -->
    <script src="assets/vendor/jquery/jquery-1.12.4.js"></script>
    <script src="assets/js/doT.js"></script>
</head>

<body>
<!--显示区-->

<!--js模板-->

<!--数据源-->

<!--模板渲染-->
</body>

</html>
```

**下面的例子中使用的都是写死的数据，具体项目中数据源使用的是Ajax从后台获取数据**



### 使用教程

#### 1. 赋值

**格式**

```js
{{= }}
```

**Demo**

```js
<!--显示区-->
<div id="view"></div>

<!--js模板-->
<script type="text/x-dot-template" id="js-template">
    <h1>Hello, {{=it.name}}!</h1>
</script>

<!--数据源-->
<script>
    let json = {
        "name": "秋叶依剑",
        "age": "23"
    };
</script>

<!--模板渲染-->
<script>
    let tmpl = doT.template($("#js-template").html());        /*生成模板方法*/
    $("#view").html(tmpl(json));                //数据渲染
</script>
```



#### 2. 数组

**格式**

```js
{{~it:value:index }}
    ...
{{~}}
```

**Demo**

```html
 <!--显示区-->
    <div id="view"></div>

    <!--js模板-->
    <script type="text/x-dot-template" id="js-template">
        <ul>
            {{~it:value:index}}
            <li>姓名: {{=value.name}} 年龄: {{=value.age}}</li>
            {{~}}
        </ul>
    </script>

    <!--数据源-->
    <script>
        let json = [{
            "name": "张三",
            "age": 21
        }, {
            "name": "李四",
            "age": 22
        }, {
            "name": "王五",
            "age": 23
        }];
    </script>

    <!--模板渲染-->
    <script>
        let tmpl = doT.template($("#js-template").html()); /*生成模板方法*/
        $("#view").html(tmpl(json)); //数据渲染
    </script>
```



#### 3. 条件

**格式**

```js
{{? value.age >= 100}}       <!--if(value.age == 100)-->
	<li>姓名: {{=value.name}} 年龄: 百岁老人</li>
{{?? value.age <= 3}}       <!--else if(value.age == 3)-->
	<li>姓名: {{=value.name}} 年龄: 可爱baby</li>
{{??}}                      <!--else-->
	<li>姓名: {{=value.name}} 年龄: {{=value.age}}</li>
{{?}}                       <!--结束条件判断-->
```

**Demo**

```html
    <!--显示区-->
    <div id="view"></div>

    <!--js模板-->
    <script type="text/x-dot-template" id="js-template">
        <ul>
            {{~it:value:index}}
                {{? value.age >= 100}}       <!--if(value.age == 100)-->
                    <li>姓名: {{=value.name}} 年龄: 百岁老人</li>
                {{?? value.age <= 3}}       <!--else if(value.age == 3)-->
                    <li>姓名: {{=value.name}} 年龄: 可爱baby</li>
                {{??}}                      <!--else-->
                    <li>姓名: {{=value.name}} 年龄: {{=value.age}}</li>
                {{?}}                       <!--结束条件判断-->
            {{~}}       <!--结束数组循环-->
        </ul>

    </script>

    <!--数据源-->
    <script>
        let json = [{
            "name": "张三",
            "age": 21
        }, {
            "name": "李四",
            "age": 100
        }, {
            "name": "王五",
            "age": 3
        }];
    </script>

    <!--模板渲染-->
    <script>
        let tmpl = doT.template($("#js-template").html()); /*生成模板方法*/
        $("#view").html(tmpl(json)); //数据渲染
    </script>
```



#### 4. 编码渲染

将获取的数据以字符串的形式渲染，html标签不会被浏览器解析，可以防止代码注入

**格式**

```js
{{! }}
```

**Demo**

```html
 <!--显示区-->
    <div id="view"></div>

    <!--js模板-->
    <script type="text/x-dot-template" id="js-template">
        <ul>
            {{~it:value:index}}
                {{? value.age == 20}}
                    <!--按照原来的样式形式输出，html标签会被浏览器解析-->
                    <li>姓名: {{=value.name}} FreeStyle: {{=value.freeStyle}}</li>
                {{??}}
                    <!--字符串的形式输出，html标签不会被浏览器解析，可以防止代码注入-->
                    <li>姓名: {{=value.name}} FreeStyle: {{!value.freeStyle}}</li>
                {{?}}
            {{~}}
        </ul>
    </script>

    <!--数据源-->
    <script>
        let json = [{
            "name": "张三",
            "age": 20,
            "freeStyle": "<h3>Rap</h3>"
        }, {
            "name": "李四",
            "age": 18,
            "freeStyle": "<h3>Basketball</h3>"
        }];
    </script>

    <!--模板渲染-->
    <script>
        let tmpl = doT.template($("#js-template").html()); /*生成模板方法*/
        $("#view").html(tmpl(json)); //数据渲染
    </script>
```



#### 5. 定义与引用公共模块

**格式**

```html
 <!--定义公共模块1-->
{{##def.module1:
	...
#}}

<!--引用模块1-->
{{#def.module1}}
```

**Demo**

```html
 <!--显示区-->
    <div id="view"></div>

    <!--js模板-->
    <script type="text/x-dot-template" id="js-template">
        <!--定义公共模块1-->
        {{##def.module1:
            <li style="color: red">姓名: {{=value.name}} FreeStyle: {{=value.freeStyle}} <span>你引用了模块1</span></li>
        #}}
        <!--定义公共模块2-->
        {{##def.module2:
            <li style="color: pink">姓名: {{=value.name}} FreeStyle: {{=value.freeStyle}} <span>你引用了模块2</span></li>
        #}}

        <ul>
            {{~it:value:index}}
                {{? value.age == 18}}
                    <!--引用模块1-->
                    {{#def.module1}}
                {{?? value.age == 20}}
                    <!--引用模块2-->
                    {{#def.module2}}
                {{?}}
            {{~}}
        </ul>


    </script>

    <!--数据源-->
    <script>
        let json = [{
            "name": "张三",
            "age": 20,
            "freeStyle": "Rap"
        }, {
            "name": "李四",
            "age": 18,
            "freeStyle": "BasketBall"
        }];
    </script>

    <!--模板渲染-->
    <script>
        let tmpl = doT.template($("#js-template").html()); /*生成模板方法*/
        $("#view").html(tmpl(json)); //数据渲染
    </script>
```



#### 6. 循环

**格式**

**每行JS语句均需要 `{{ }}`包住**

```js
{{ for(let i = 0; i < it.length; i++){ }}
	{{ let value = it[i]; }}
	{{ if(value.age === 20) { }}
     	...
     {{ }else{ }}
          ...
     {{ } }}
{{ } }}
```

**Demo**

```html
   <!--显示区-->
    <div id="view"></div>

    <!--js模板-->
    <script type="text/x-dot-template" id="js-template">
        <!--定义公共模块1-->
        {{##def.module1:
            <li style="color: red">姓名: {{=value.name}} FreeStyle: {{=value.freeStyle}} <span>你引用了模块1</span></li>
        #}}
        <!--定义公共模块2-->
        {{##def.module2:
            <li style="color: pink">姓名: {{=value.name}} FreeStyle: {{=value.freeStyle}} <span>你引用了模块2</span></li>
        #}}

        <ul>
            {{ for(let i = 0; i < it.length; i++){ }}
                {{ let value = it[i]; }}
                {{ if(value.age === 20) { }}
                    {{#def.module1}}
               {{ }else{ }}
                    {{#def.module2}}
                {{ } }}
            {{ } }}
        </ul>

    </script>

    <!--数据源-->
    <script>
        let json = [{
            "name": "张三",
            "age": 20,
            "freeStyle": "Rap"
        }, {
            "name": "李四",
            "age": 18,
            "freeStyle": "BasketBall"
        }];
    </script>

    <!--模板渲染-->
    <script>
        let tmpl = doT.template($("#js-template").html()); /*生成模板方法*/
        $("#view").html(tmpl(json)); //数据渲染
    </script>
```



### Reference

* http://www.jq22.com/jquery-info8648
* https://www.cnblogs.com/zhao-yi/p/7485447.html



