## HTML技术问题总结

### 1.链接方式 引入CSS

* 在HTML 头部的 <head> 标签引入外部的 CSS 文件。

``` html
<head>
    <link rel="stylesheet" type="text/css" href="style.css">
</head>
```

### 2. 链接方式 引入JS

``` js
<script type="text/javascript" src="js/index.js" ></script>
```

### 3. 标签总结

```html
<b></b> : 加粗
<i></i> : 斜体
<strong></strong> : 加粗，包含语义
<em></em> : 斜体，包含语义

注：包含语义的标签对搜索引擎更友好
```
```html
<ul type = "circle">	//无序列表，常见为type属性
    <li></li>
    <li></li>
</ul>
```

```html
<ol type = "1">	//有序列表
    <li></li>
    <li></li>
</ol>
```

```html
<table>
    <!--
常用属性：
	border: 指定边框
	width： 宽度
	height：高度
	bgcolor: 背景颜色
	align : 对齐方式

	colspan: 跨列
	rowspan: 跨行
	-->    
    <tr> //第一行
    	<td>第一列</td>
        <td>第二列</td> 
    </tr>
    <tr> //第二行
    	<td>第一列</td>
        <td>第二列</td>
    </tr>
    <tr> //第三行
    	<td>第一列</td>
        <td>第二列</td>
    </tr>
       
    
</table>
```

```html
<img />

<!--
常用属性：
	src : 路径
	width ：宽度
	height ： 高度
	alt : 图片加载错误时的提示信息
-->
```

```html
<input />

type: 指定输入项的类型
	text : 文本
	password : 密码框
	radio : 单选按钮
	checkbox : 复选框
	file : 上传文件
	submit : 提交按钮
	button : 普通提交按钮
	reset : 重置按钮
	hidden : 隐藏域
	
	date : 日期类型
	tel : 手机号
	number : 只允许输入数字
	
	palaceholder : 指定默认提示信息
	name : 在表单提交的时候， 当做参数的名称
	id : 给输入项取一个名字，以便后期我们去找到它，并操作它
```

```html
textarea : 文本域，可以输入一段文本
	cols : 指定宽度
	rows : 指定高度
```

```html
select : 下拉列表
	option ： 选择项
```





### 5. 路径问题

```HTML
./ : 当前路径
../ :上一级路径
../../ : 
```



### 6. < form> 表单的novalidate 属性

 定义和用法

novalidate 属性是一个布尔属性。

novalidate 属性规定当提交表单时不对表单数据（输入）进行验证。

```html
<form action="demo_form.html" novalidate>
	E-mail: <input type="email" name="user_email" required>
			<input type="submit">
</form>
```

