jQuery 选择选取元素

 jQuery选择器

ID选择器（js一般尽量用ID选择器，效率最高） 

​          $("#id").html();

 类选择器 

​         $(".className").text();

标签选择器 

​         $('p').click()

属性选择器

​          $("li[id]")、 $("li[id='link']").fadeIn();

 层级选择器 

​         $("li .link").show();

父子选择器

​       $("ul > li")

 伪类选择器

​        $("p:first")

​         $("ul li:eq(3)")

表单选择器

​          $(":text")

​          $(":checkbox")

​          $(":checked")

选择器汇总

\*   $("*")     所有元素

\#id    $("#lastname")   id="lastname" 的元素

.class   $(".intro")     所有 class="intro" 的元素

element   $("p")   所有 <p> 元素

.class.class  $(".intro.demo")  所有 class="intro" 且 class="demo" 的元素

:first $("p:first")  第一个 <p> 元素

:last  $("p:last")   最后一个 <p> 元素

:even  $("tr:even")  所有偶数 <tr> 元素

:odd  $("tr:odd")   所有奇数 <tr> 元素

:eq(index)   $("ul li:eq(3)")  列表中的第四个元素（index 从 0 开始）

:gt(no)     $("ul li:gt(3)")  列出 index 大于 3 的元素 greater than

:lt(no)     $("ul li:lt(3)")  列出 index 小于 3 的元素 less than

:not(selector) $("input:not(:empty)") 所有不为空的 input 元素

:header     $(":header")    所有标题元素 <h1> - <h6>

 :animated    所有动画元素

:contains(text)   $(":contains('W3School')") 包含指定字符串的所有元素

:empty       $(":empty")         无子（元素）节点的所有元素

:hidden       $("p:hidden")        所有隐藏的 <p> 元素

:visible      $("table:visible")     所有可见的表格

s1,s2,s3       $("th,td,.intro")      所有带有匹配选择的元素

[attribute]     $("[href]")     所有带有 href 属性的元素

[attribute=value]  $("[href='#']")   所有 href 属性的值等于 "#" 的元素

[attribute!=value] $("[href!='#']")  所有 href 属性的值不等于 "#" 的元素

[attribute$=value] $("[href$='.jpg']") 所有 href 属性的值包含以 ".jpg" 结尾的元素

:input   $(":input")     所有 <input> 元素

:text    $(":text")     所有 type="text" 的 <input> 元素

:password  $(":password")   所有 type="password" 的 <input> 元素

:radio   $(":radio")     所有 type="radio" 的 <input> 元素

:checkbox  $(":checkbox")   所有 type="checkbox" 的 <input> 元素

:submit   $(":submit")    所有 type="submit" 的 <input> 元素

:reset   $(":reset")     所有 type="reset" 的 <input> 元素 

:button   $(":button")    所有 type="button" 的 <input> 元素

 :image   $(":image")     所有 type="image" 的 <input> 元素

:file    $(":file")     所有 type="file" 的 <input> 元素

:enabled  $(":enabled")  所有激活的 input 元素 

:disabled  $(":disabled") 所有禁用的 input 元素

:selected  $(":selected") 所有被选取的 input 元素

:checked  $(":checked")  所有被选中的 input 元素

 

 jQuery选择方法

获取父级元素

$(selector).parent();   //获取直接父级

 $(selector).parents('p'); //获取所有父级元素直到html  

 

 获取子代和后代的元素

$(selector).children();  //获取直接子元素

$(selector).find("span"); //获取所有的后代元素

 find方法 可能用的多。

 

获取同级的元素

$(selector).siblings()  //所有的兄弟节点

$(selector).next()    //下一个节点

$(selector).nextAll()   //后面的所有节点

$(selector).prev()    //前面一个的兄弟节点

 $(selector).prevAll()   //前面的所有的兄弟节点

 

过滤方法

$("div p").last();    //取最后一个元素

$("div p").first();    //取第一个元素

$("p").eq(1);       //去第n个元素

$("p").filter(".intro"); //过滤，选择所有p标签带有 .intro类

$("p").not(".intro");   //去除，跟上面的filetr正好相反