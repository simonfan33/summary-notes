# CSS的使用

> CSS  层叠样式表(英文全称：(Cascading Style Sheets)   能够对网页中元素位置的排版进行像素级精确控制，支持几乎所有的字体字号样式，拥有对网页对象和模型样式编辑的能力 ,简单来说,美化页面

## 3.1 CSS引入方式

> 行内式,通过元素开始标签的style属性引入, 样式语法为       样式名:样式值; 样式名:样式值;

- 代码

```html
    <input 
        type="button" 
        value="按钮"
        style="
            display: block;
            width: 60px; 
            height: 40px; 
            background-color: rgb(140, 235, 100); 
            color: white;
            border: 3px solid green;
            font-size: 22px;
            font-family: '隶书';
            line-height: 30px;
            border-radius: 5px;
    "/> 
```

- 效果

![1681201322584](C:/Users/89585/Desktop/%E5%B0%9A%E7%A1%85%E8%B0%B7%E5%AD%A6%E4%B9%A0%E6%96%87%E4%BB%B6/MySQL/day06-html/2023%E5%B9%B47%E6%9C%883%E6%97%A5/%E7%AC%94%E8%AE%B0/images/1681201322584.png)

- 缺点
  - html代码和css样式代码交织在一起,增加阅读难度和维护成本
  - css样式代码仅对当前元素有效,代码重复量高,复用度低

> 内嵌式

- 代码

```html
<head>
    <meta charset="UTF-8">
    <style>
        /* 通过选择器确定样式的作用范围 */
        input {
            display: block;
            width: 80px; 
            height: 40px; 
            background-color: rgb(140, 235, 100); 
            color: white;
            border: 3px solid green;
            font-size: 22px;
            font-family: '隶书';
            line-height: 30px;
            border-radius: 5px;
        }
    </style>
</head>
<body>
    <input type="button" value="按钮1"/> 
    <input type="button" value="按钮2"/> 
    <input type="button" value="按钮3"/> 
    <input type="button" value="按钮4"/> 
</body>
```



- 效果

![1681201553427](C:/Users/89585/Desktop/%E5%B0%9A%E7%A1%85%E8%B0%B7%E5%AD%A6%E4%B9%A0%E6%96%87%E4%BB%B6/MySQL/day06-html/2023%E5%B9%B47%E6%9C%883%E6%97%A5/%E7%AC%94%E8%AE%B0/images/1681201553427.png)



- 说明
  - 内嵌式样式需要在head标签中,通过一对style标签定义CSS样式
  - CSS样式的作用范围控制要依赖选择器
  - CSS的样式代码中注释的方式为  /*   */
  - 内嵌式虽然对样式代码做了抽取,但是CSS代码仍然在html文件中
  - 内嵌样式仅仅能作用于当前文件,代码复用度还是不够,不利于网站风格统一

> 连接式/外部样式表

- 可以在项目单独创建css样式文件,专门用于存放CSS样式代码

<img src="C:/Users/89585/Desktop/%E5%B0%9A%E7%A1%85%E8%B0%B7%E5%AD%A6%E4%B9%A0%E6%96%87%E4%BB%B6/MySQL/day06-html/2023%E5%B9%B47%E6%9C%883%E6%97%A5/%E7%AC%94%E8%AE%B0/images/1681202361429.png" alt="1681202361429" style="zoom: 67%;" />



- 在head标签中,通过link标签引入外部CSS样式即可

```html
<head>
    <meta charset="UTF-8">
    <link href="css/buttons.css" rel="stylesheet" type="text/css"/>
</head>
<body>
    <input type="button" value="按钮1"/> 
    <input type="button" value="按钮2"/> 
    <input type="button" value="按钮3"/> 
    <input type="button" value="按钮4"/> 
</body>

```

- 说明
  - CSS样式代码从html文件中剥离,利于代码的维护
  - CSS样式文件可以被多个不同的html引入,利于网站风格统一

## 3.2 CSS选择器

> 元素选择器

- 代码

```html
<head>
    <meta charset="UTF-8">
   <style>
    input {
        display: block;
        width: 80px; 
        height: 40px; 
        background-color: rgb(140, 235, 100); 
        color: white;
        border: 3px solid green;
        font-size: 22px;
        font-family: '隶书';
        line-height: 30px;
        border-radius: 5px;
    }
   </style>
</head>
<body>
    <input type="button" value="按钮1"/> 
    <input type="button" value="按钮2"/> 
    <input type="button" value="按钮3"/> 
    <input type="button" value="按钮4"/> 
    <button>按钮5</button>
</body>
```

- 效果

![1681203591080](C:/Users/89585/Desktop/%E5%B0%9A%E7%A1%85%E8%B0%B7%E5%AD%A6%E4%B9%A0%E6%96%87%E4%BB%B6/MySQL/day06-html/2023%E5%B9%B47%E6%9C%883%E6%97%A5/%E7%AC%94%E8%AE%B0/images/1681203591080.png)

- 说明
  - 根据标签名确定样式的作用范围
  - 语法为  元素名 {}
  - 样式只能作用到同名标签上,其他标签不可用
  - 相同的标签未必需要相同的样式,会造成样式的作用范围太大

> id选择器

- 代码

```html
<head>
    <meta charset="UTF-8">
   <style>
    #btn1 {
        display: block;
        width: 80px; 
        height: 40px; 
        background-color: rgb(140, 235, 100); 
        color: white;
        border: 3px solid green;
        font-size: 22px;
        font-family: '隶书';
        line-height: 30px;
        border-radius: 5px;
    }
   </style>
</head>
<body>
    <input id="btn1" type="button" value="按钮1"/> 
    <input id="btn2" type="button" value="按钮2"/> 
    <input id="btn3" type="button" value="按钮3"/> 
    <input id="btn4" type="button" value="按钮4"/> 
    <button id="btn5">按钮5</button>
</body>
```

- 效果

![1681203748017](C:/Users/89585/Desktop/%E5%B0%9A%E7%A1%85%E8%B0%B7%E5%AD%A6%E4%B9%A0%E6%96%87%E4%BB%B6/MySQL/day06-html/2023%E5%B9%B47%E6%9C%883%E6%97%A5/%E7%AC%94%E8%AE%B0/images/1681203748017.png)

- 说明
  - 根据元素id属性的值确定样式的作用范围
  - 语法为  #id值 {}
  - id属性的值在页面上具有唯一性,所有id选择器也只能影响一个元素的样式
  - 因为id属性值不够灵活,所以使用该选择器的情况较少

> class选择器

- 代码

```html
<head>
    <meta charset="UTF-8">
   <style>
    .shapeClass {
        display: block;
        width: 80px; 
        height: 40px; 
        border-radius: 5px;
    }
    .colorClass{
        background-color: rgb(140, 235, 100); 
        color: white;
        border: 3px solid green;
    }
    .fontClass {
        font-size: 22px;
        font-family: '隶书';
        line-height: 30px;
    }

   </style>
</head>
<body>
    <input  class ="shapeClass colorClass fontClass"type="button" value="按钮1"/> 
    <input  class ="shapeClass colorClass" type="button" value="按钮2"/> 
    <input  class ="colorClass fontClass" type="button" value="按钮3"/> 
    <input  class ="fontClass" type="button" value="按钮4"/> 
    <button class="shapeClass colorClass fontClass" >按钮5</button>
</body>

```



- 效果

![1681204269702](C:/Users/89585/Desktop/%E5%B0%9A%E7%A1%85%E8%B0%B7%E5%AD%A6%E4%B9%A0%E6%96%87%E4%BB%B6/MySQL/day06-html/2023%E5%B9%B47%E6%9C%883%E6%97%A5/%E7%AC%94%E8%AE%B0/images/1681204269702.png)

- 说明
  - 根据元素class属性的值确定样式的作用范围
  - 语法为  .class值 {}
  - class属性值可以有一个,也可以有多个,多个不同的标签也可以是使用相同的class值
  - 多个选择器的样式可以在同一个元素上进行叠加
  - 因为class选择器非常灵活,所以在CSS中,使用该选择器的情况较多



## 3.4 CSS浮动

> CSS 的 Float（浮动）使元素脱离文档流，按照指定的方向（左或右发生移动），直到它的外边缘碰到包含框或另一个浮动框的边框为止。

- 浮动设计的初衷为了解决文字环绕图片问题，浮动后一定不会将文字挡住，这是设计初衷。
- 文档流是是文档中可显示对象在排列时所占用的位置/空间，而脱离文档流就是在页面中不占位置了。  

> 浮动原理

- 当把框 1 向右浮动时，它脱离文档流并且向右移动，直到它的右边缘碰到包含框的右边缘

<img src="C:/Users/89585/Desktop/%E5%B0%9A%E7%A1%85%E8%B0%B7%E5%AD%A6%E4%B9%A0%E6%96%87%E4%BB%B6/MySQL/day06-html/2023%E5%B9%B47%E6%9C%883%E6%97%A5/%E7%AC%94%E8%AE%B0/images/1681260732580.png" alt="1681260732580" style="zoom: 80%;" />

- 当框 1 向左浮动时，它脱离文档流并且向左移动，直到它的左边缘碰到包含框的左边缘。因为它不再处于文档流中，所以它不占据空间，实际上覆盖住了框 2，使框 2 从视图中消失。如果把所有三个框都向左移动，那么框 1 向左浮动直到碰到包含框，另外两个框向左浮动直到碰到前一个浮动框。



<img src="C:/Users/89585/Desktop/%E5%B0%9A%E7%A1%85%E8%B0%B7%E5%AD%A6%E4%B9%A0%E6%96%87%E4%BB%B6/MySQL/day06-html/2023%E5%B9%B47%E6%9C%883%E6%97%A5/%E7%AC%94%E8%AE%B0/images/1681260842005.png" alt="1681260842005" style="zoom: 80%;" />

- 如果包含框太窄，无法容纳水平排列的三个浮动元素，那么其它浮动块向下移动，直到有足够的空间。如果浮动元素的高度不同，那么当它们向下移动时可能被其它浮动元素“卡住”

<img src="C:/Users/89585/Desktop/%E5%B0%9A%E7%A1%85%E8%B0%B7%E5%AD%A6%E4%B9%A0%E6%96%87%E4%BB%B6/MySQL/day06-html/2023%E5%B9%B47%E6%9C%883%E6%97%A5/%E7%AC%94%E8%AE%B0/images/1681260887708.png" alt="1681260887708" style="zoom: 80%;" />

> 浮动的样式名:float

<img src="C:/Users/89585/Desktop/%E5%B0%9A%E7%A1%85%E8%B0%B7%E5%AD%A6%E4%B9%A0%E6%96%87%E4%BB%B6/MySQL/day06-html/2023%E5%B9%B47%E6%9C%883%E6%97%A5/%E7%AC%94%E8%AE%B0/images/1681260937920.png" alt="1681260937920" style="zoom:80%;" />

> 通过代码感受浮动的效果

- 代码

```html
<head>
    <meta charset="UTF-8">
   <style>
    .outerDiv {
        width: 500px;
        height: 300px;
        border: 1px solid green;
        background-color: rgb(230, 224, 224);
    }
    .innerDiv{
        width: 100px;
        height: 100px;
        border: 1px solid blue;
        float: left;
    }
    .d1{
        background-color: greenyellow;
       /*  float: right; */
    }
    .d2{
        background-color: rgb(79, 230, 124);
    }
    .d3{
        background-color: rgb(26, 165, 208);
    }
   </style>
</head>
<body>
   <div class="outerDiv">
        <div class="innerDiv d1">框1</div>
        <div class="innerDiv d2">框2</div>
        <div class="innerDiv d3">框3</div>
   </div> 
</body>
```

- 效果

<img src="C:/Users/89585/Desktop/%E5%B0%9A%E7%A1%85%E8%B0%B7%E5%AD%A6%E4%B9%A0%E6%96%87%E4%BB%B6/MySQL/day06-html/2023%E5%B9%B47%E6%9C%883%E6%97%A5/%E7%AC%94%E8%AE%B0/images/1681261311289.png" alt="1681261311289" style="zoom: 67%;" />



## 3.5 CSS定位

> position 属性指定了元素的定位类型。

- 这个属性定义建立元素布局所用的定位机制。任何元素都可以定位，不过绝对或固定元素会生成一个块级框，而不论该元素本身是什么类型。相对定位元素会相对于它在正常流中的默认位置偏移。
- 元素可以使用的顶部，底部，左侧和右侧属性定位。然而，这些属性无法工作，除非是先设定position属性。他们也有不同的工作方式，这取决于定位方法。

<img src="C:/Users/89585/Desktop/%E5%B0%9A%E7%A1%85%E8%B0%B7%E5%AD%A6%E4%B9%A0%E6%96%87%E4%BB%B6/MySQL/day06-html/2023%E5%B9%B47%E6%9C%883%E6%97%A5/%E7%AC%94%E8%AE%B0/images/1681261377875.png" alt="1681261377875" style="zoom: 80%;" />

> 静态定位

- 说明
  - 不设置的时候的默认值就是static，静态定位，没有定位，元素出现在该出现的位置，块级元素垂直排列，行内元素水平排列
- 代码

```html
<head>
    <meta charset="UTF-8">
    <style>
        .innerDiv{
                width: 100px;
                height: 100px;
        }
        .d1{
            background-color: rgb(166, 247, 46);
            position: static;
        }
        .d2{
            background-color: rgb(79, 230, 124);
        }
        .d3{
            background-color: rgb(26, 165, 208);
        }
    </style>
</head>
<body>
        <div class="innerDiv d1">框1</div>
        <div class="innerDiv d2">框2</div>
        <div class="innerDiv d3">框3</div>
</body>
```



- 效果

<img src="C:/Users/89585/Desktop/%E5%B0%9A%E7%A1%85%E8%B0%B7%E5%AD%A6%E4%B9%A0%E6%96%87%E4%BB%B6/MySQL/day06-html/2023%E5%B9%B47%E6%9C%883%E6%97%A5/%E7%AC%94%E8%AE%B0/images/1681261602297.png" alt="1681261602297" style="zoom:50%;" />



> 绝对定位 

- 说明
  - absolute ,通过 top left right bottom 指定元素在页面上的固定位置
  - 定位后元素会让出原来位置,其他元素可以占用
- 代码

```html
<head>
    <meta charset="UTF-8">
    <style>
        .innerDiv{
                width: 100px;
                height: 100px;
        }
        .d1{
            background-color: rgb(166, 247, 46);
            position: absolute;
            left: 300px;
            top: 100px;
        }
        .d2{
            background-color: rgb(79, 230, 124);
        }
        .d3{
            background-color: rgb(26, 165, 208);
        }
    </style>
</head>
<body>
        <div class="innerDiv d1">框1</div>
        <div class="innerDiv d2">框2</div>
        <div class="innerDiv d3">框3</div>
</body>
```



- 效果

<img src="C:/Users/89585/Desktop/%E5%B0%9A%E7%A1%85%E8%B0%B7%E5%AD%A6%E4%B9%A0%E6%96%87%E4%BB%B6/MySQL/day06-html/2023%E5%B9%B47%E6%9C%883%E6%97%A5/%E7%AC%94%E8%AE%B0/images/1681261830830.png" alt="1681261830830" style="zoom:50%;" />

> 相对定位

- 说明
  - relative 相对于自己原来的位置进行地位
  - 定位后保留原来的站位,其他元素不会移动到该位置
- 代码

```html
<head>
    <meta charset="UTF-8">
    <style>
        .innerDiv{
                width: 100px;
                height: 100px;
        }
        .d1{
            background-color: rgb(166, 247, 46);
            position: relative;
            left: 30px;
            top: 30px;
        }
        .d2{
            background-color: rgb(79, 230, 124);
        }
        .d3{
            background-color: rgb(26, 165, 208);
        }
    </style>
</head>
<body>
        <div class="innerDiv d1">框1</div>
        <div class="innerDiv d2">框2</div>
        <div class="innerDiv d3">框3</div>
</body>
```



- 效果

<img src="C:/Users/89585/Desktop/%E5%B0%9A%E7%A1%85%E8%B0%B7%E5%AD%A6%E4%B9%A0%E6%96%87%E4%BB%B6/MySQL/day06-html/2023%E5%B9%B47%E6%9C%883%E6%97%A5/%E7%AC%94%E8%AE%B0/images/1681261993904.png" alt="1681261993904" style="zoom:50%;" />



> 固定定位

- 说明
  - fixed 失踪在浏览器窗口固定位置,不会随着页面的上下移动而移动
  - 元素定位后会让出原来的位置,其他元素可以占用
- 代码

```html
<head>
    <meta charset="UTF-8">
    <style>
        .innerDiv{
                width: 100px;
                height: 100px;
        }
        .d1{
            background-color: rgb(166, 247, 46);
            position: fixed;
            right: 30px;
            top: 30px;
        }
        .d2{
            background-color: rgb(79, 230, 124);
        }
        .d3{
            background-color: rgb(26, 165, 208);
        }
    </style>
</head>
<body>
        <div class="innerDiv d1">框1</div>
        <div class="innerDiv d2">框2</div>
        <div class="innerDiv d3">框3</div>
        br*100+tab
</body>
```

- 效果

<img src="C:/Users/89585/Desktop/%E5%B0%9A%E7%A1%85%E8%B0%B7%E5%AD%A6%E4%B9%A0%E6%96%87%E4%BB%B6/MySQL/day06-html/2023%E5%B9%B47%E6%9C%883%E6%97%A5/%E7%AC%94%E8%AE%B0/images/fixeddingwei.gif" alt="fixeddingwei" style="zoom:50%;" />

## 3.6 CSS盒子模型

> 所有HTML元素可以看作盒子，在CSS中，"box model"这一术语是用来设计和布局时使用。

- CSS盒模型本质上是一个盒子，封装周围的HTML元素，它包括：边距（margin），边框（border），填充（padding），和实际内容（content）

<img src="C:/Users/89585/Desktop/%E5%B0%9A%E7%A1%85%E8%B0%B7%E5%AD%A6%E4%B9%A0%E6%96%87%E4%BB%B6/MySQL/day06-html/2023%E5%B9%B47%E6%9C%883%E6%97%A5/%E7%AC%94%E8%AE%B0/images/1681262535006.png" alt="1681262535006" style="zoom:67%;" />

- 说明：
  - Margin(外边距) - 清除边框外的区域，外边距是透明的。
  - Border(边框) - 围绕在内边距和内容外的边框。
  - Padding(内边距) - 清除内容周围的区域，内边距是透明的。
  - Content(内容) - 盒子的内容，显示文本和图像。

<img src="C:/Users/89585/Desktop/%E5%B0%9A%E7%A1%85%E8%B0%B7%E5%AD%A6%E4%B9%A0%E6%96%87%E4%BB%B6/MySQL/day06-html/2023%E5%B9%B47%E6%9C%883%E6%97%A5/%E7%AC%94%E8%AE%B0/images/1681262585852.png" alt="1681262585852" style="zoom:67%;" />

- 代码

```html
    <head>
        <meta charset="UTF-8">
       <style>
        .outerDiv {
            width: 800px;
            height: 300px;
            border: 1px solid green;
            background-color: rgb(230, 224, 224);
            margin: 0px auto;
        }
        .innerDiv{
            width: 100px;
            height: 100px;
            border: 1px solid blue;
            float: left;
            /* margin-top: 10px;
            margin-right: 20px;
            margin-bottom: 30px;
            margin-left: 40px; */
            margin: 10px 20px 30px 40px;
           
        }
        .d1{
            background-color: greenyellow;
            /* padding-top: 10px;
            padding-right: 20px;
            padding-bottom: 30px;
            padding-left: 40px; */
            padding: 10px 20px 30px 40px;
        }
        .d2{
            background-color: rgb(79, 230, 124);
        }
        .d3{
            background-color: rgb(26, 165, 208);
        }
       </style>
    </head>
    <body>
       <div class="outerDiv">
            <div class="innerDiv d1">框1</div>
            <div class="innerDiv d2">框2</div>
            <div class="innerDiv d3">框3</div>
       </div> 
    </body>
```

- 效果

<img src="C:/Users/89585/Desktop/%E5%B0%9A%E7%A1%85%E8%B0%B7%E5%AD%A6%E4%B9%A0%E6%96%87%E4%BB%B6/MySQL/day06-html/2023%E5%B9%B47%E6%9C%883%E6%97%A5/%E7%AC%94%E8%AE%B0/images/1681263227281.png" alt="1681263227281" style="zoom: 67%;" />



- 在浏览器上,通过F12工具查看盒子模型状态

<img src="C:/Users/89585/Desktop/%E5%B0%9A%E7%A1%85%E8%B0%B7%E5%AD%A6%E4%B9%A0%E6%96%87%E4%BB%B6/MySQL/day06-html/2023%E5%B9%B47%E6%9C%883%E6%97%A5/%E7%AC%94%E8%AE%B0/images/1681263265604.png" alt="1681263265604" style="zoom: 67%;" />

