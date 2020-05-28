## JAVASCRIPT



### 1. 获取地址栏中参数



```javascript
function getAddressURLParam(paramName)
{
	 //构造一个含有目标参数的正则表达式的对象
	 var reg = new RegExp("(^|&)" + paramName + "=([^&]*)(&|$)");
	 //匹配目标参数
	 var url = window.location.search.substr(1).match(reg);
	//返回参数值
	if(url != null)
	 return unescape(url[2]);
	return null;
}
```



### 2. 动态获取元素id 属性

```html
<a href="java:void(0)" onclick="GetID(this)" class="CSS" id="变量">文字</a> 
```

```js
<script type="text/javascript">
    function GetID(obj) {
        var ID = obj.getAttribute("id");
        alert(ID);
    }
</script> 

```

