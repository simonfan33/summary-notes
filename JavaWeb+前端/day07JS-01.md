## 2、代码

```javascript
<body>

    <button id="btn">点我，有本事你点我！</button>
    
</body>
<!-- 为什么放在 body 标签后面？ -->
<!-- 因为浏览器按顺序加载HTML标签，如果script标签在button标签前面， -->
<!-- 那么script标签中JavaScript代码执行时，就无法加载button标签对应的元素对象。 -->
<script type="text/javascript">
    // 1、查找按钮对象
    var btnEle = document.getElementById("btn");

    // ※在浏览器的控制台打印 btnEle 变量的值
    console.log("btnEle="+btnEle);

    // 2、声明一个函数，函数每次执行时都弹出一个alert()警告框
    function showAlert() {
        // ※alert()是系统内置的函数，可以直接调用
        alert("HelloWorld");
    }

    // 3、给按钮对象绑定单击事件响应函数
    // ※语法格式：元素对象.事件属性 = 响应函数的引用
    // ※注意：函数的名称就是函数的引用，后面千万不要加括号
    // ※函数名（或函数引用）后面加括号表示调用、执行这个函数
    btnEle.onclick = showAlert;

</script>
```

# 二、基本语法

## 1、数据类型

### ①基本数据类型

- 字符串：JavaScript中不区分字符和字符串，单引号、双引号定义的都是字符串。
- 数值类型：JavaScript中不区分整数和小数。
- 布尔类型：true和false

### ②引用类型

- 对象
- 数组
- 函数：JavaScript中函数也是一种特殊的对象
- 正则表达式

### ③类型之间的转换

- 字符串转换为布尔类型的规则是：非空字符串转换为true，空字符串转换为false
- 数值类型转换为布尔类型：非零的数值（哪怕是负数）：转换为true，为零的数值：转换为false
- 字符串可以自动转换为数值：在数学运算中转换
  - 但是注意：+号和字符串在一起会被理解为字符串连接

### ④两个特殊值

- NaN：not a number，意思是“非数字”，用来表示当前计算结果不是数字。
- undefined：未定义。

## 2、变量

- ES6之前的语法规则中，使用var关键词声明变量。
- ES6开始的语法规则中，除了var，还可以使用let关键词声明变量。
- 一个变量声明之后，可以赋值任何类型。
- 一个变量在使用过程中，仍然可以赋值各种不同的类型。
- JavaScript中标识符严格区分大小写。
- 在声明变量之前打印变量不会报错，只是返回undefined。

<br/>

建议：我们继续延续我们Java中开发的编码习惯即可。

<br/>

## 3、分支结构

注意：其它数据类型可以转回为布尔类型，例如："false"坑。<br/>

## 4、循环结构

```javascript
// 通过 for ... in 循环结构获取数组的各个下标
for(var index in arr) {
    // 根据下标从数组中读取对应的值
    var value = arr[index];
    console.log("for in value="+value);
}

console.log("===============");

var obj = new Object();
obj.objId = 5;
obj.objName = "tom";
obj.objAge = 20;

// 通过 for ... in 循环结构获取对象中每一个属性名
for(var propName in obj) {
    // 根据属性名从对象中读取对应的属性值
    console.log(propName + "=" + obj[propName]);
}
```

# 二、函数

## 1、系统内置函数

- alert()：弹出一个警告框。
- confirm()：弹出一个确认框。
- prompt()：弹出输入框
- isNaN()：判断传入的参数是否是非数字

## 2、用户自定义函数

### ①基本用法

```javascript
// 1、声明函数
// 形参列表只需要指定变量名，不需要写类型或var关键词
function sum(i, j) {
    return i+j;
}

// 2、调用函数
var sumResult = sum(5, 6);
console.log("sumResult="+sumResult);

// 3、以另一种方式声明函数
// 函数引用赋值给一个变量，此时函数本身不需要命名
// 没有起名字的函数，我们叫匿名函数
var showMessage = function(){
    console.log("I am a message.");
};

// 4、调用函数：变量名就相当于函数名
showMessage();
```

### ②神经病用法

```javascript
// 5、把函数名赋值给另一个变量
var printMessage = showMessage;

// 6、继续使用变量名作为函数名调用函数
printMessage();

// 7、把变量名作为另一个函数的参数传进去
function doPrintMessage(funName) {
    // 在函数名（或者说函数的引用）后面加括号表示调用函数
    funName();
}

// 8、把指向函数对象的变量作为参数传入doPrintMessage(funName)函数
doPrintMessage(printMessage);

// 9、声明一个函数，函数的返回值也是一个函数
function returnFun() {
    return function() {
        console.log("I am a very good message.");
    };
}

// 10、调用函数返回的函数
console.log(returnFun());
returnFun()();

// 11、原地调用一个匿名函数
(function (){
    alert("我已经疯了！");
})();
```

## 3、this关键字

函数中的this关键字指向调用函数的那个对象。

```javascript
// 12、声明一个函数测试this关键字
function showName() {
    console.log(this.stuName);
}

var stu01 = new Object();
stu01.stuName = "peter";

var stu02 = new Object();
stu02.stuName = "mary";

// 函数的引用赋值给对象的属性
stu01.showName = showName;
stu02.showName = showName;

// 调用函数
stu01.showName();
stu02.showName();
```

# 三、对象

## 1、创建对象

```javascript
// 1、创建对象方式一
var obj01 = new Object();

// 3、创建对象方式二
var obj02 = {
    "weaponId":666,
    "weaponName":"大狙",
    "weaponPrice":1000.22
};
```

## 2、给对象属性赋值

```javascript
// 2、给对象属性赋值就直接操作即可
obj01.soldierId = 5;
obj01.soldierName = "Jack";
obj01.soldierJob = "cooker";
```

# 四、数组

```javascript
// 1、创建数组方式一
var arr01 = new Array();
console.log(arr01);

// 2、创建数组方式二
var arr02 = ["aaa", "bbb", "ccc"];
console.log(arr02);

// 3、调用push()方法向数组存入元素（相当于堆栈操作中的压栈）
arr01.push("apple");
arr01.push("banana");
arr01.push("orange");
console.log(arr01);

arr02.push("ddd");
arr02.push("eee");
arr02.push("fff");
console.log(arr02);

// 4、和压栈对应的操作是弹栈
var popValue = arr02.pop();
console.log(popValue);
console.log(arr02);

// 5、调用reverse()方法把数组元素倒序排列
arr02.reverse();
console.log(arr02);

// 6、把数组元素连成字符串
var arr02Str = arr02.join(",");
console.log(arr02Str);

// 7、把字符串根据指定的符号拆分为数组
arr02 = arr02Str.split(",");
console.log(arr02);

// 8、在指定索引位置删除指定个数的数组元素
arr02.splice(2, 2);
console.log(arr02);

// 9、数组切片
arr02.push("uuu", "vvv", "www", "xxx", "yyy", "zzz");
console.log(arr02);

// 从 start 参数指定的索引开始，到 end 参数指定的索引结束
// 选取半闭半开区间进行切片（也就是说不包括end索引指向的元素）
var arr03 = arr02.slice(2, 5);
console.log(arr03);
```

# 五、JSON格式

## 1、语法

- JSON格式边界的符号只有两种：
  - {}表示这个JSON数据是一个JSON对象
  - []表示这个JSON数据是一个JSON数组
- JSON对象的语法
  - {}中是由多组key:value对组成的
  - key:value对之间用逗号分开
  - key和value之间用冒号分开
  - key固定就是字符串类型
- JSON数组的语法
  - []中是由多组value组成的
  - value之间用逗号分开
- value的类型：不管在JSON对象还是JSON数组中，value类型都适用下面的规则
  - value可以是基本数据类型
  - value可以是引用数据类型
    - JSON对象
    - JSON数组

<br/>

```json
# JSON对象
{"stuId":10, "stuName":"tom"}
# JSON数组
["aaa", "bbb", "ccc"]
# 嵌套的JSON对象
{
    "stuId":10, 
    "stuName":"tom",
    "school": {
        "schoolName": "atguigu",
        "schoolAge": 10,
        "subjectArr": ["Java", "H5", "Big Data"]
    }
}
# 嵌套的JSON数组
[
    {
        "shenzhen":"number one"
    },
    {
        "wuhan":"number two"
    },
    {
        "shanghai":"number three"
    }
]
```

## 2、JSON对象转字符串

```javascript
JSON.stringify(jsonObj);
```

## 3、字符串转JSON对象

```javascript
JSON.parse(jsonStr);
```

# 六、引入方式

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>JavaScript引入方式</title>
</head>
<body>

    <!-- 通常不建议使用，因为这种方式意味着结构和行为耦合。 -->
    <button onclick="alert('Hello!');">引入方式一：内嵌到HTML标签</button>

    <!-- JavaScript 代码仅限于当前页面使用。 -->
    <button id="btn01">引入方式二：内嵌到script标签</button>

    <!-- JavaScript 代码不局限于当前页面使用。 -->
    <!-- 或引入其它JavaScript库或框架。 -->
    <button id="btn02">引入方式三：引入外部JS文件</button>
    
</body>
<script>
    document.getElementById("btn01").onclick = function(){alert("HelloWorld!")};
</script>

<!-- 使用 src 属性指定外部 JS 文件的路径 -->
<!-- 如果script标签是用来引入外部 JS 文件，那么script标签内部什么代码都不要写 -->
<script src="script/work.js">
    alert("我是来捣乱的！");
</script>

<!-- 如果script标签是用来引入外部 JS 文件，那么script标签不要当作单标签结束 -->
<!-- <script src="script/work.js"/> -->
</html>
```

# 