## Css 笔记



### 1. box-shadow

```css
box-shadow: 10px 10px 5px rgba(0, 0, 0, 0.1);
```

> box-shadow: *h-shadow v-shadow blur spread color* inset;

| 值         | 说明                                                         |
| :--------- | :----------------------------------------------------------- |
| *h-shadow* | 必需的。水平阴影的位置。允许负值                             |
| *v-shadow* | 必需的。垂直阴影的位置。允许负值                             |
| *blur*     | 可选。模糊距离                                               |
| *spread*   | 可选。阴影的大小                                             |
| *color*    | 可选。阴影的颜色。在[CSS颜色值](https://www.runoob.com/cssref/css_colors_legal.aspx)寻找颜色值的完整列表 |
| inset      | 可选。从外层的阴影（开始时）改变阴影内侧阴影                 |



### 2. 引入外部样式

```css
<link href="assets/css/style.css" rel="stylesheet">
```

rel 属性规定当前文档与被链接文档之间的关系。在上面面的例子中，rel 属性指示被链接的文档是一个样式表



### 3. 一个页面模板



```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta content="width=device-width, initial-scale=1.0" name="viewport">

    <title>页面标准模板</title>
    <meta name="keywords" content="网站关键字">
    <meta name="description" content="网站描述">

<!--    Favicon-->
    <link href="assets/img/favicon.png" rel="icon">

<!--    Css File-->
    <link href="assets/css/style.css" rel="stylesheet">

</head>
<body>
<h1>Hello, world!</h1>




<!--  JS File -->
<script src="assets/js/main.js"></script>
</body>
</html>
```





### 4. margin

 属性定义及使用说明

margin简写属性在一个声明中设置所有外边距属性。该属性可以有1到4个值。

实例:

- margin:10px 5px 15px 20px;
  - 上边距是 10px
  - 右边距是 5px
  - 下边距是 15px
  - 左边距是 20px
- margin:10px 5px 15px;
  - 上边距是 10px
  - 右边距和左边距是 5px
  - 下边距是 15px
- margin:10px 5px;
  - 上边距和下边距是 10px
  - 右边距和左边距是 5px
- margin:10px;
  - 所有四个边距都是 10px

### 5. white-space

white-space属性指定元素内的空白怎样处理。

```css
white-space:nowrap;
```

 属性值

| 值       | 描述                                                         |
| :------- | :----------------------------------------------------------- |
| normal   | 默认。空白会被浏览器忽略。                                   |
| pre      | 空白会被浏览器保留。其行为方式类似 HTML 中的 <pre> 标签。    |
| nowrap   | 文本不会换行，文本会在在同一行上继续，直到遇到 <br> 标签为止。 |
| pre-wrap | 保留空白符序列，但是正常地进行换行。                         |
| pre-line | 合并空白符序列，但是保留换行符。                             |
| inherit  | 规定应该从父元素继承 white-space 属性的值。                  |



### 6. text-transform

text-transform 属性控制文本的大小写。

```css
text-transform:uppercase;
```

 属性值

| 值         | 描述                                           |
| :--------- | :--------------------------------------------- |
| none       | 默认。定义带有小写字母和大写字母的标准的文本。 |
| capitalize | 文本中的每个单词以大写字母开头。               |
| uppercase  | 定义仅有大写字母。                             |
| lowercase  | 定义无大写字母，仅有小写字母。                 |
| inherit    | 规定应该从父元素继承 text-transform 属性的值。 |



### 7. text-decoration

text-decoration 属性规定添加到文本的修饰，下划线、上划线、删除线等。



```css
text-decoration: none;                     /*没有文本装饰*/
text-decoration: underline red;            /*红色下划线*/
text-decoration: underline wavy red;       /*红色波浪形下划线*/
```

属性值

| 值           | 描述                                            |
| :----------- | :---------------------------------------------- |
| none         | 默认。定义标准的文本。                          |
| underline    | 定义文本下的一条线。                            |
| overline     | 定义文本上的一条线。                            |
| line-through | 定义穿过文本下的一条线。                        |
| blink        | 定义闪烁的文本。                                |
| inherit      | 规定应该从父元素继承 text-decoration 属性的值。 |



### 8. text-transform





### 9. :after 选择器

定义和用法

:after 选择器在被选元素的内容后面插入内容。

请使用 content 属性来指定要插入的内容。

```css
p:after
{ 
content:"台词：-";
background-color:yellow;
color:red;
font-weight:bold;
}
```



### 10. 浮动

加了浮动之后的元素，会具有很多特性

1. 浮动元素会脱离标准流

   * 脱离标准普通流的控制（浮）移动到指定位置（动），俗称脱标
   * 浮动的盒子不再保留原先的位置

2. 浮动元素会一行内显示并且元素顶部对齐

3. 浮动的元素会具有行内块元素的特性

   任何元素都可以浮动，不管原先是什么模式的元素，添加浮动之后具有行内块元素相似特性

   * 如果块级元素没有设置宽度，默认宽度和父级一样宽，但是添加浮动之后，他的大小根据内容来决定
   * 浮动的盒子中间是没有缝隙的，是紧挨着的
   * 行内元素同理

* 清除浮动的本质
  * 清除浮动的本质是清除浮动元素造成的影响
  * 如果父盒子本身有高度，则不需要清除浮动
  * 清除浮动之后，父级就会根据浮动的子盒子自动检测高度，父级有了高度，就不会影响下面的标准流了

* 清除浮动的做法

  * 额外标签法(不常用)

    在最后一个浮动的子元素后面添加一个额外的标签

    ```
    .clear {
    	clear:both;
    }
    ```

  * 父级添加overflow

    ```
    overflow: hidden;
    ```

  * :after 伪元素

    给父级元素添加下面的样式

    ```css
    .clearfix:after {
        content: "";
        display: block;
        height: 0;
        clear: both;
        visibility: hidden;
    }
    .clearfix {			/*IE6、7专有*/
        *zoom: 1;
    }
    ```

  * 双伪元素清除浮动

    也是给父级元素添加

    ```css
    .clearfix:before, .clearfix:after {
        content: "";
        display: table;
    }
    .clearfix:after {
        clear:both;
    }
    .clearfix {
        *zoom:1;
    }
    ```

### 11. 定位

1. 浮动可以让多个块级盒子一行没有缝隙显示，经常用于横向排列盒子
2. 定位则是可以让盒子自由地在某个盒子内移动位置或者固定屏幕某个位置，并且可以压住其他盒子
3. 相对定位（relative）是元素在移动位置的时候，是相对于它原来的位置来说的（自恋型）
   * 移动位置的时候参照点是自己原来的位置
   * 原来在标准流的位置继续占有，后面的盒子仍然以标准流的方式来对待它。（不脱标）

4. 绝对定位是元素在移动位置的时候，是相对于它祖先元素来说的（拼爹型）
   * 如果没有祖先元素或者祖先元素没有定位，则以浏览器为准（document文档）

> 让父级元素相对定位，就可以是让级元素不仅有定位还可以不脱离标准流。这样子元素使用绝对定位时就可以以父级元素为准而不是以浏览器为准



### 12. ！importance

在非IE浏览器中 后面加上`!important` 的样式优先级更高，而ie浏览器不识别`!important`

```
.colortest { 
border:20px  solid #60A179 !important;
border:20px  solid #00F;
padding: 30px;
width : 300px;
} 
```





### 13. white-space

规定段落中的文本不进行(xing)换行(hang)操作



### 14. visibility

定义和用法

visibility 属性规定元素是否可见。

**提示：**即使不可见的元素也会占据页面上的空间。请使用 "display" 属性来创建不占据页面空间的不可见元素。



### 15. z-index

属性定义及使用说明

z-index 属性指定一个元素的堆叠顺序。

拥有更高堆叠顺序的元素总是会处于堆叠顺序较低的元素的前面。

也就是说，z-index 的值大的会压在小的上面

**注意：** z-index 进行定位元素(position:absolute, position:relative, or position:fixed)



### 16.  transition-timing-function 属性

 属性定义及使用说明

transition-timing-function属性指定切换效果的速度。

此属性允许一个过渡效果，以改变其持续时间的速度。



语法

```
transition-timing-function: linear|ease|ease-in|ease-out|ease-in-out|cubic-bezier(*n*,*n*,*n*,*n*);
```



| 值                            | 描述                                                         |
| :---------------------------- | :----------------------------------------------------------- |
| linear                        | 规定以相同速度开始至结束的过渡效果（等于 cubic-bezier(0,0,1,1)）。 |
| ease                          | 规定慢速开始，然后变快，然后慢速结束的过渡效果（cubic-bezier(0.25,0.1,0.25,1)）。 |
| ease-in                       | 规定以慢速开始的过渡效果（等于 cubic-bezier(0.42,0,1,1)）。  |
| ease-out                      | 规定以慢速结束的过渡效果（等于 cubic-bezier(0,0,0.58,1)）。  |
| ease-in-out                   | 规定以慢速开始和结束的过渡效果（等于 cubic-bezier(0.42,0,0.58,1)）。 |
| cubic-bezier(*n*,*n*,*n*,*n*) | 在 cubic-bezier 函数中定义自己的值。可能的值是 0 至 1 之间的数值。 |



### 17. flex-wrap 属性

flex-wrap 属性规定flex容器是单行或者多行，同时横轴的方向决定了新行堆叠的方向。

**注意：**如果元素不是弹性盒对象的元素，则 flex-wrap 属性不起作用。

也就是必要的时候可以自动换行 



###18. text-indent: 2em

首行缩进2字符



### 18.  justify-content 属性

justify-content 用于设置或检索弹性盒子元素在主轴（横轴）方向上的对齐方式。

**提示：**使用 align-content 属性对齐交叉轴上的各项（垂直）。

属性值

| 值            | 描述                                                         | 测试                                                         |
| :------------ | :----------------------------------------------------------- | :----------------------------------------------------------- |
| flex-start    | 默认值。项目位于容器的开头。                                 | [测试 »](https://www.runoob.com/try/playit.php?f=playcss_justify-content&preval=flex-start) |
| flex-end      | 项目位于容器的结尾。                                         | [测试 »](https://www.runoob.com/try/playit.php?f=playcss_justify-content&preval=flex-end) |
| center        | 项目位于容器的中心。                                         | [测试 »](https://www.runoob.com/try/playit.php?f=playcss_justify-content&preval=center) |
| space-between | 项目位于各行之间留有空白的容器内。                           | [测试 »](https://www.runoob.com/try/playit.php?f=playcss_justify-content&preval=space-between) |
| space-around  | 项目位于各行之前、之间、之后都留有空白的容器内。             | [测试 »](https://www.runoob.com/try/playit.php?f=playcss_justify-content&preval=space-around) |
| initial       | 设置该属性为它的默认值。请参阅 [*initial*](https://www.runoob.com/cssref/css-initial.html)。 | [测试 »](https://www.runoob.com/try/playit.php?f=playcss_justify-content&preval=initial) |
| inherit       | 从父元素继承该属性。请参阅 [*inherit*](https://www.runoob.com/cssref/css-inherit.html)。 |                                                              |



### 19. linear-gradient() 函数

定义与用法

linear-gradient() 函数用于创建一个线性渐变的 "图像"。

为了创建一个线性渐变，你需要设置一个起始点和一个方向（指定为一个角度）的渐变效果。你还要定义终止色。终止色就是你想让Gecko去平滑的过渡，并且你必须指定至少两种，当然也会可以指定更多的颜色去创建更复杂的渐变效果。

css语法

```css
background-image: linear-gradient(direction, color-stop1, color-stop2, ...);
```

默认方向从上到下

css做一个渐变图片背景

```css
background-image: linear-gradient(to right bottom, rgba(0, 0, 0, 0), rgba(0, 0, 0, 0)), url("");
```

