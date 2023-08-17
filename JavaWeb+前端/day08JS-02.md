# 七、BOM

## 1、概念

BOM：Browser Object Model浏览器对象模型。这是为了方便我们在JavaScript程序中操作浏览器的特定功能而封装的一系列对象。
<br/>

## 2、思想

现实世界的事物要体现到IT系统中，就需要把现实世界的事物进行抽象，抽取现实事物的主要特征，然后在IT系统中创建一个模型来对应。模型在代码中通过对象来体现。<br/>
比如：现实世界中的一个人，我们开发员工档案管理系统就只关注这个人作为员工的这方面。<br/>
在员工信息中，我们提取下面数据到程序中：

- 员工编号
- 员工姓名
- 员工籍贯
- 员工所在部门

<br/>

有了这些信息就可以声明一个类来代表现实世界的员工。

<br/>

总结：现实世界事物 --> 抽象出模型 --> 封装为对象 --> 通过对象操作数据

## 3、操作举例

```javascript
<body>

    <button id="btn01">点我去尚硅谷学习！</button>
    <button id="btn02">点我去尚硅谷学习！[省略了href属性]</button>
    
</body>
<script>
    // 1、系统内置了window对象代表浏览器窗口，可以直接使用
    // 2、读操作
    // window.location.href返回浏览器地址栏当前的 URL 地址
    console.log(window.location.href);

    // 3、写操作
    document.getElementById("btn01").onclick=function(){
        window.location.href="http://www.atguigu.com";
    };

    document.getElementById("btn02").onclick=function(){
        window.location="http://www.atguigu.com";
    };

</script>
```

# 八、DOM [重点]

## 1、概念

DOM：Document Object Model文档对象模型。<br/>

- 模型：把整个HTML文档作为一个模型
- 对象：用window.document对象来代表HTML文档
- 浏览器加载机制：浏览器把HTML文档加载到内存的过程中，每读取一个HTML标签就创建一个元素对象，这个元素对象会存入document对象中。

## 2、节点

HTML文档中所有对象都被看作节点（Node）。节点在进一步细分：

- 元素节点（Element）：对应HTML标签
- 属性节点（Attr）：对应HTML标签中的属性
- 文本节点（Text）：对应文本标签体

<br/>

> 这几个类型之间是父子关系。Element、Attr、Text都可以看做是Node的子类。<br/>
> 这个说法是帮助我们理解，实际上JavaScript这门语言中没有“类”的概念。

<br/>

## 3、DOM树

### ①元素之间的关系

- 纵向：
  - 父子关系
  - 先辈和后代的关系
- 横向：兄弟关系

<br/>

### ②元素和属性的关系

把HTML标签封装为元素对象，HTML标签的属性就是元素对象的属性。<br/>

### ③元素和文本的关系

元素对象和它里面的文本节点对象也是父子关系。<br/>

## 4、元素查找

### ①整个文档范围内查找

使用document对象调用getElementByXxx()方法就是在整个文档范围内查找。

- document.getElementById("id值") 返回id值对应的单个对象。
- document.getElementsByTagName("标签名") 返回标签名对应的多个对象。
- document.getElementsByName("表单标签的name属性值") 根据表单标签的name属性值查询对象。

<br/>

### ②指定元素范围内查找

- 全部子节点：元素对象.childNodes
- 全部子元素：元素对象.children
- 第一个子元素：元素对象.firstElementChild
- 最后一个子元素：元素对象.lastElementChild

<br/>

### ③查找元素的父元素

- 方式一：parentElement
- 方式二：parentNode

### ④查找元素的兄弟元素

- 前一个兄弟元素：previousElementSibling
- 后一个兄弟元素：nextElementSibling

## 5、读写元素属性

- 读操作：元素对象.属性名
- 写操作：元素对象.属性名=新属性值

## 6、读写文本标签体

### ①innerText属性

- 读操作：元素对象.innerText
- 写操作：元素对象.innerText=新文本值
  注意：新文本值中如果包含HTML标签代码，不会被解析

### ②innerHTML属性

- 读操作：元素对象.innerHTML
- 写操作：元素对象.innerHTML=新文本值
  注意：新文本值中如果包含HTML标签代码，能够被解析

# 九、事件驱动 [重点]

## 1、类比

| 生活中的例子     | 事件驱动                                 |
| ---------------- | ---------------------------------------- |
| 地雷             | 事件响应函数                             |
| 兵工厂生产地雷   | 声明事件响应函数                         |
| 找到埋地雷的位置 | 把绑定事件响应函数的元素对象给查出来     |
| 埋地雷           | 把事件响应函数绑定到元素上               |
| 触发引信         | 用户在浏览器窗口进行操作的时候触发了事件 |
| 地雷爆炸         | 事件响应函数执行                         |

## 2、事件类型

### ①文本框值改变事件

```javascript
// 1、查找文本框元素对象
var inputEle = document.getElementsByName("username")[0];

// 2、声明事件响应函数
function showUsername() {
    // 我们希望在这个函数中获取到用户输入的新值
    // this 在事件响应函数中代表触发事件的元素对象
    console.log("用户输入的新数据：" + this.value);
}

// 3、事件绑定
// 事件类型：值改变
// ※特殊说明：值改变事件要触发需要满足两个条件
// 条件1：当前控件失去焦点
// 条件2：值确实发生了改变
inputEle.onchange = showUsername;
```

### ②失去焦点

```javascript
// 事件类型：失去焦点
inputEle.onblur = function() {
    console.log("文本框失去了焦点。");
};
```

### ③鼠标移动事件

HTML设置：

```html
<img id="dongGe" src="images/wanghaodong.jpg" />
```

<br/>

CSS样式设置：

```css
img {
    display: none;
}
```

<br/>

JavaScript代码：

```javascript
// 1、获取div元素对象
var divEle = document.getElementById("swimmingPool");
var imgEle = document.getElementById("dongGe");

// 2、绑定事件响应函数
divEle.onmousemove = function() {
    // 3、打印事件对象
    // console.log(event);

    // 4、通过事件对象获取鼠标的坐标
    // console.log(event.clientX + " " + event.clientY);

    // 5、把鼠标坐标信息写入div中，作为它的文本值
    this.innerText = event.clientX + " " + event.clientY;

    // 6、让图片显示
    imgEle.style = "display:block;position:absolute;top:"+(event.clientY+10)+"px;left:"+(event.clientX+10)+"px;";
};
```

## 3、取消控件默认行为

### ①提出问题

- 删除记录的超链接：点击后我们希望弹出确认框，用户如果点击取消，则不跳转页面。
- 表单验证：点击提交按钮之后，执行表单验证，表单验证如果通不过，那么就不提交表单。

### ②默认行为

- 超链接点击之后会跳转页面
- 表单的提交按钮点击之后会提交表单

### ③取消默认行为实现方式

```javascript
// 调用事件对象的方法
event.preventDefault();
```

## 4、取消事件冒泡

### ①概念

每一个元素处理完事件（事件响应函数执行完）之后，默认会把事件向上传递给自己的父元素，父元素如果没有绑定对应类型的事件响应函数，那么就什么都不会发生。如果父元素绑定了对应的事件响应函数，就会导致父元素再处理一遍这个事件

### ②阻止的方式

```javascript
event.stopPropagation();
```

## 5、回调函数

### ①普通函数

我们自己声明一个函数，我们自己调用。<br/>

### ②回调函数

我们自己声明一个函数，交给系统来调用。<br/>

```javascript
// 这里我们声明的函数，仅仅只是声明了，我们自己并没有在后面的代码中去调用它。
// 函数的引用赋值给onclick属性之后，我们就没有再管它了
// 浏览器在监听到用户单击的事件之后，才调用这个函数
document.getElementById("outerDiv").onclick = function() {
    alert("你点击了【外层】div");
};
```

<br/>

### ③Java中的回调函数

```java
public class RunnableTest{

    public static void main(String[] args){
        Runnable runnable = new Runnable(){

            // 我们声明了这个方法，但是真正调用的时候是系统来调用的
            public void run() {
                System.out.println(Thread.currentThread().getName());
            }
        };

        // 调用 start() 方法启动线程
        new Thread(runnable).start();
    }

}
```

<br/>

# 十、正则表达式

## 1、概念

根据功能的需求，在原始字符串数据中，把我们想要的匹配出来。

## 2、应用场景

- 模式验证：用正则表达式规定一个格式，检查字符串是否满足这个格式。比如：检查手机号、邮箱、身份证号……
- 匹配读取：在原始字符串数据中，把我们需要的字符串读取出来。
- 匹配替换：在原始字符串数据中，把匹配固定模式的字符串替换成指定的数据。

## 3、创建正则表达式

### ①对象形式

```javascript
var source = "Hello tom,good afternoon.";

// 1、创建正则表达式对象
// 方式一：对象方式
// reg 是 regular 的缩写，意思是正规的、常规的
// exp 是 expression 的缩写，意思是表达式
// 在 RegExp 构造器中传入正则表达式本身
// "a" 正则表达式表示：被检测的字符串，有a这个字符就匹配，否则不匹配
var reg = new RegExp("a");

var checkResult = reg.test(source);
console.log("checkResult = " + checkResult);
```

### ②直接量形式

```javascript
// 方式二：直接量方式
// 类似的，JSON 对象、JSON 数组也相当于是用直接量的方式创建
// {"userName":"tom"} ["a","b","c"]
reg = /w/;
checkResult = reg.test(source);
console.log("checkResult = " + checkResult);
```

## 4、正则表达式语法

### ①正则表达式的组成

#### [1]普通字符

用字符本身进行匹配。

```javascript
reg = /Hello/;
checkResult = reg.test(source);
console.log("checkResult = " + checkResult);
```

#### [2]元字符

使用一些在正则表达式中有特定含义的字符，来检测字符串

```javascript
// ^ 表示：要求目标字符串以指定内容开头
// ^T 表示：要求目标字符串以T开头
reg = /^T/;
checkResult = reg.test(source);
console.log("checkResult = " + checkResult);
```

| 代码 | 说明                                                         |
| ---- | ------------------------------------------------------------ |
| .    | 匹配除换行字符以外的任意字符。                               |
| \w   | 匹配字母或数字或下划线等价于[a-zA-Z0-9 _]                    |
| \W   | 匹配任何非单词字符。等价于[^A-Za-z0-9 _]                     |
| \s   | 匹配任意的空白符，包括空格、制表符、换页符等等。等价于[\f\n\r\t\v]。 |
| \S   | 匹配任何非空白字符。等价于[^\f\n\r\t\v]。                    |
| \d   | 匹配数字。等价于[0-9]。                                      |
| \D   | 匹配一个非数字字符。等价于[^0-9]                             |
| \b   | 匹配单词的开始或结束                                         |
| ^    | 匹配字符串的开始，但在[]中使用表示取反                       |
| $    | 匹配字符串的结束                                             |

### ②字符集合

如果我们想要设定的规则，是一组字符中的某一个，那么就可以使用字符集合来设置。字符集合用[]定义。

#### [1]匹配列表中任何一个

```javascript
reg = /[oda]/;
checkResult = reg.test(source);
console.log("checkResult = " + checkResult);

reg = /[xyz]/;
checkResult = reg.test(source);
console.log("checkResult = " + checkResult);
```

#### [2]匹配列表外任何一个

```javascript
// [^oda]表示：目标字符串不能包含o、d、a中的任何一个
reg = /[^oda]/;
checkResult = reg.test(source);
console.log("checkResult = " + checkResult);
```

#### [3]用字符范围定义集合

```javascript
// [A-Z]表示：包含大写字母中的任何一个即可
// [a-z]表示：包含小写字母中的任何一个即可
// [0-9]表示：包含数字中的任何一个即可
// [A-Za-z]表示：包含字母中的任何一个即可
// [A-Za-z0-9]表示：包含字母、数字中的任何一个即可
reg = /[A-Z]/;
checkResult = reg.test(source);
console.log("checkResult = " + checkResult);
```

### ③字符出现次数

| 代码  | 说明           |
| ----- | -------------- |
| *     | 出现零次或多次 |
| +     | 出现一次或多次 |
| ?     | 出现零次或一次 |
| {n}   | 出现n次        |
| {n,}  | 出现n次或多次  |
| {n,m} | 出现n到m次     |

## 5、举例

### ①格式验证

注意：此时是使用正则表达式对象调用方法

```javascript
var source = "Hello tom!Good morning!";
var reg = /W/;
var checkResult = reg.test(source);
console.log("checkResult = " + checkResult);

reg = /G/;
checkResult = reg.test(source);
console.log("checkResult = " + checkResult);
```

### ②匹配读取

注意：此时是使用字符串对象调用方法

```javascript
reg = /[A-Z]/;
var matchArray = source.match(reg);
console.log(matchArray);
```

### ③替换

注意：此时是使用字符串对象调用方法

```javascript
reg = /o/;
var replaceResult = source.replace(reg, "*");
console.log(replaceResult);
```

### ④全文查找

```javascript
reg = /[A-Z]/g;
matchArray = source.match(reg);
console.log(matchArray);

reg = /o/g;
replaceResult = source.replace(reg, "*");
console.log(replaceResult);
```

### ⑤忽略大小写

```javascript
reg = /[A-Z]/gi;
matchArray = source.match(reg);
console.log(matchArray);
```

### ⑥使用元字符

```javascript
reg = /^H/;
checkResult = reg.test(source);
console.log("checkResult = " + checkResult);

reg = /^U/;
checkResult = reg.test(source);
console.log("checkResult = " + checkResult);

// ^H\.$表示要求目标字符串以H开头，以点结尾。
// ^符号必须写在前面，$符号必须写在后面
// \.表示对.进行转义，因为.本身是匹配任意一个字符
// .*表示任意字符可以出现零次或多次
reg = /^H.*\.$/;
checkResult = reg.test(source);
console.log("checkResult = " + checkResult);
```

### ⑦使用字符集合

```javascript
var splitArr = source.split(" ");
reg = /^[a-z].*[a-z]$/;
for (var i = 0; i < splitArr.length; i++) {
    var value = splitArr[i];
    console.log("value = " + value + " " + reg.test(value));
}
```

