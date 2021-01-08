# JavaScript核心+新特性

> 课程名称: [从零玩转JavaScript核心+新特性③](https://www.it666.com/my/course/183)
>
> 课程概述: ECMAScript核心语法, DOM文档对象模型, BOM文档对象模型
>
> 讲述人: 李南江

## 第 1 章: JavaScript开篇

### 1.1 什么是JavaScript

JavaScript简称JS，是前端开发的一门脚本语言(解释型语言)

**解释型语言：**程序执行之前，不需要对代码进行编译，在运行时边解析边执行的语言

[浏览器工作原理](https://www.html5rocks.com/en/tutorials/internals/howbrowserswork/)

![image-20210108165340842](assets/image-20210108165340842.png)

**编译型语言：**程序执行之前，需要一个专门的编译过程，把程序编译成机器语言的文件，比如exe文件

![image-20210108165357260](assets/image-20210108165357260.png)

### 1.2 JavaScript作用

HTML 提供网页上显示的内容（结构）

![image-20210108165652201](assets/image-20210108165652201.png)

CSS 美化网页（样式）

![image-20210108165705313](assets/image-20210108165705313.png)

avaScript 控制网页行为（行为）

![image-20210108165721361](assets/image-20210108165721361.png)

### 1.3 JavaScript发展史

JavaScript起源于Netscape公司的LiveScript语言

- 1994年网景公司发布历史上第一个比较成熟的浏览器(Navigator 0.9), 但是只能浏览不能***交互\***
- 1995年为了解决表单有效性验证就要与服务器进行多次地往返交互问题，网景公司录用Brendan Eich(布兰登·艾奇)，他在 10 天内开发出 LiveScript 语言
- 在 Netscape Navigator 2.0 即将正式发布前，Netscape 将LiveScript 更名为 JavaScript， 目的是为了蹭Java的热度
- 所以Java和 JavaScript之间的关系就像老婆和老婆饼一样

[参考文献](http://www.w3school.com.cn/js/pro_js_history.asp)



### 1.4 JavaScript组成

**ECMAScript** ：JavaScript的语法标准 *[ECMAScript起源](http://www.w3school.com.cn/js/pro_js_history.asp)*

- ECMA是European Computer Manufacturers Association的缩写，即欧洲计算机制造商协会
- ECMAScript是ECMA制定的脚本语言的标准, 规定了一种脚本语言实现应该包含的基本内容
- JavaScript是脚本语言的一种,所以JavaScript也必须遵守ECMAScript标准,包含ECMAScript标准中规定的基本内容

**DOM(Document Object Model)** ：JavaScript操作网页上的元素(标签)的API

**BOM(Browser Object Model)** ：JavaScript操作浏览器的部分功能的API

![image-20210108165912789](assets/image-20210108165912789.png)

## 第 2 章: ECMAScript 

### 2.1 ECMAScript快速入门

#### 2.1.1 书写格式

css书写格式: 

```
1. 行内样式: 写在标签内部
2. 内嵌样式(内联样式) : 写在一对head标签中
3. 外链样式: 写在一个单独的.css文件中, 再导入进来
```

**JavaScript书写格式**

- 行内样式: 写在标签内部

- 内嵌样式(内联样式) : 写在一对head标签中
- 外链样式: 写在一个单独的.js文件中, 再导入进来

##### 行内式格式(不推荐)

```html
<div onclick="alert('hello world');">我是div</div>
```

##### 内嵌式格式

```html
</body>
<!-- ... ... -->
<script type="text/javascript">
       alert("hello world");
</script>
</body>
```

##### 外链式格式

```html
<script type="text/javascript" src="01-js书写格式.js"></script>
```

##### 书写格式注意点

1. 不推荐直接将JavaScript代码书写到标签内部
2. 默认情况下浏览器会从上至下的解析网页, 所以如果将JavaScript写到一对head标签中, 并且需要通过JavaScript代码操作界面上的元素, 那么就不能直接书写JavaScript代码, 否则无效
   - 如果想将JavaScript写到一对head标签中, 并且需要在JavaScript代码中操作界面上的元素, 那么必须加上`window.onload = function(){操作界面元素的JavaScript}`
   - window.onload的含义: 等到界面上所有的内容都加载完毕再执行{}中的代码
   - 由于默认情况下浏览器会从上至下的解析网页, 所以如果想通过JavaScript操作界面上的元素只需要等到元素被加载解析之后操作就可以了, 所以我们还可以将JavaScript代码写到body结束标签的前面
3. 如果通过外链式导入.js文件, 并且需要在.js文件中操作界面上的元素, 那么如果是在head标签中导入的, 必须在.js文件中加上window.onload. 如果是在body结束标签前面导入的, 那么就不用添加window.onload
4. 如果需要在一对script标签中编写JavaScript代码, 那么就不能同时通过script标签再导入其它的.js文件, 否则书写的JavaScript代码无效



#### 2.1.2 常见输出方式

##### 通过弹窗的形式来输出

```javascript
/*
alert(需要输出的内容);
confirm(需要输出的内容);
prompt(需要输出的内容);
*/
alert("hello world");
prompt("请输入内容：");
confirm("你好吗?");
```

**注意点:**

- 如果需要输出的内容不是数字, 那么就必须通过单引号或者双引号括起来
- 在JavaScript中是严格区分大小写的, alert()和ALERT()不是一回事
- 在编写JavaScript代码的时候, 一定要记住每一句代码后面都需要添加一个分号, 并且这个分号必须是英文的分号
- 我们会发现有时候不写分号程序也能够运行, 这里并不是因为不需要分号, 而是浏览器自动帮助我们添加了分号, 浏览器自动添加会消耗一定的性能, 并且有可能会添加错误



##### 页面中显示内容

```javascript
// document.write(需要输出的内容);
document.write("hello world2");
```

注意点: 如果需要输出的内容不是数字, 那么就必须通过单引号或者双引号括起来

##### 通过开发者工具控制台的形式来输出

```javascript
/*
	console.log(需要输出的内容);  // 普通输出
    console.warn(需要输出的内容); // 警告输出
    console.error(需要输出的内容); // 错误输出
*/
console.log("hello world3");
console.error("错误信息");
console.warn("警告信息");
```

![image-20210108170540249](assets/image-20210108170540249.png)

注意点: 如果需要输出的内容不是数字, 那么就必须通过单引号或者双引号括起来

#### 2.1.3 常量

##### 什么是常量

- 常量表示一些固定不变的数据
- 现实生活中人的性别其实就可以看做是常量, 生下来是男孩一辈子都是男孩, 生下来是女孩一辈子都是女孩

##### JavaScript中常量的分类

###### 整型常量

整型常量其实就是正数, 在JavaScript中随便写一个整数都是整型常量

```javascript
 1   /   666  /    99
```

###### 实型常量

实型常量其实就是小数, 在JavaScript中随便写一个小数都是实型常量

```javascript
3.14   6.66
```

###### 字符串常量

字符串常量其实就是用单引号或者双引号括起来的内容, 我们就称之为字符串常量

```javascript
'a'
'abc'
"1"
"知播渔教育"
```

注意点: 无论用单引号或者双引号括起来了多少个字符, 在JavaScript中都是字符串常量

###### 布尔常量

布尔常量其实就是真或者假, 在JavaScript中通过true和false来表达

 在JavaScript中布尔常量只有两个取值, 真(true)或者假(false)



###### 自定义常量

在ES6中新增的

`const 常量名称 = 常量取值;`

```javascript
const NUM = 666;
// NUM = 888; //尝试修改NUM这个常量的取值(报错)
console.log(NUM);
```



#### 2.1.4 变量

##### 什么是变量

变量表示一些可以被修改的数据

在现实生活中超市的储物格就是变量, 在不同的时间段里面, 储物格中存储的数据也不一样



##### 如何定义一个变量

在JavaScript中可以通过定义变量的方式来生成储物格, 也就是告诉浏览器, 我们需要一块内存空间

`var 变量名称;`

##### 如何使用变量

使用变量就是往申请的那块内存空间中存储数据, 和获取存储的数据

###### 如何存储数据

`变量名称 = 需要存储的数据;`

可以将等号右边需要存储的数据放到等号左边变量申请的那块存储空间中

###### 如何获取存储在变量中的数据

`变量名称`

###### 如何修改变量中存储的数据

在JavaScript中想要修改变量中存储的数据, 只需要再次给变量直接赋值即可

```javascript
// 定义一个变量
var num;
// 往变量中存储数据
num = 123;
// 从变量中获取存储的数据
console.log(num);
// 修改num的值
num = 666;
console.log(num);
```



##### 变量的初始化

在JavaScript中第一次给变量赋值, 我们称之为"变量的初始化"

```javascript
var num;
num = 321; // "变量的初始化"
num = 888; // 不是"变量的初始化"
```

如果一个变量没有进行初始化, 那么变量中存储的是什么呢? 在JavaScript中如果定义了一个变量,但是没有进行初始化, 那么变量中存储的是undefined

```javascript
var num;
console.log(num);
```

给变量初始化有很多种形式

- 先定义变量, 再对变量进行初始化

  ```javascript
  var num;
  num = 321; // 先定义再初始化
  ```

- 可以在定义变量的同时对变量进行初始化

  ```javascript
  var value = 666; // 定义的同时初始化
  ```

##### 定义变量的其它格式

- 同时定义多个变量的格式

  ```javascript
  //格式: var 变量名称1, 变量名称2, .... ;
  // var num;
  // var value;
  var num, value; // 同时定义两个变量
  var num, value, value2, value3; // 同时定义四个变量
  ```

  

##### 初始化变量的其它格式

如果在企业开发中对多个变量初始化的值都是一样的, 那么我们可以通过`变量名称1 = 变量名称2 = 变量名称... = 初始化值;`

```javascript
var num;
var value;
// num = 123;
// value = 123;
num = value = 123; // 同时对num和value进行初始化, num和value中存储的数据都是123
console.log(num);
console.log(value);
```

定义多个变量的同时给多个变量分别初始化

```javascript
// var num, value;
// num = 123;
// value = 666;
var num = 123, value = 666;
console.log(num);
console.log(value);
```

##### 变量的注意点

1. 在JavaScript中变量之间是可以相互赋值的

   ```javascript
   var num;
   var value;
   num = 123;
   value = num; // 将Num中的值拷贝一份给value
   console.log(num);
   console.log(value);
   ```

2. 在JavaScript中如果定义了同名的变量, 那么后定义的变量会覆盖先定义的变量

   ```javascript
   var num = 666;
   // num = 888; // 如果num前面没有var, 那么就是修改变量中存储的值
   var num = 888;// 如果num前面有var, 那么就不是修改变量中存储的值, 而是重新定义一个新的变量
   console.log(num);
   ```

3. 在老版本的标准的(ES6之前)JavaScript中可以先使用变量, 再定义变量, 并不会报错
   由于JavaScript是一门解释型的语言, 会边解析边执行, 浏览器在解析JavaScript代码之前还会进行一个操作"预解析(预处理)"
   预解析(预处理)步骤:
   将当前JavaScript代码中所有变量的定义和函数的定义放到所有代码的最前面

   ```javascript
   console.log(num);
   var num = 123;
   /*
   预处理之后的代码
   var num;
   console.log(num); // undefined
   num = 123;
   */
   ```

##### ES6定义变量

为了解决老板标准的的两个注意点

- 在JavaScript中如果定义了同名的变量, 那么后定义的变量会覆盖先定义的变量
- 在老版本的标准的(ES6之前)JavaScript中可以先使用变量, 再定义变量, 并不会报错

在ES6中就推出了一种新的定义变量的方式

```javascript
// ES6前后定义变量的语法格式如下
var 变量名称; //ES6之前
let 变量名称; //ES6开始
```

ES6变量使用方式

```javascript
// 定义一个变量
let num;
// 给变量初始化
num = 666;
// 取出存储的数据
console.log(num);
// 修改变量中存储的数据
num = 888;
// 取出存储的数据
console.log(num);
```

ES6定义变量解决了ES6之前存在的两个问题

```javascript
// var num = 123;
// var num = 888;
let num = 123;
let num = 888; //会明确的报错
console.log(num);
```

```javascript
// console.log(num);
// var num = 123;
```



#### 2.1.5 关键字和保留字

什么是关键字？

- 被JavaScript语言赋予了特殊含义的单词

- 关键字在开发工具中会显示特殊颜色

  ```javascript
  var num;
  let num;
  ```

- 关键字不能用作变量名、函数名等

  ```javascript
  var num = 666;
  console.log(num);
  
  var break = 888; //break是关键字,定义变量会报错
  console.log(break); 
  ```

- 关键字严格区分大小写, var和Var前者是关键字, 后者不是

  ```javascript
  var num = 123;
  console.log(num);
  VAR value = 666; //VAR不可定义变量
  console.log(value);
  //只需要记住一点: 在JavaScript中所有的关键字都是小写的
  ```

保留字: 

- 使用特点和关键字都差不多,只是保留下来的可能作为关键字使用

#### 2.1.6 标识符

**什么是标识符?** 用于标识变量函数名称的

标识符命名规则(必须遵守)

- 只能由26个英文字母的大小写、10个阿拉伯数字0~9、下划线_、美元符号$组成

  ```javascript
  var abc123_$ = 678;
  console.log(abc123_$);
  
  var abc#123 = 666; //报错
  console.log(abc#123);
  
  var abc123 = 888;
  console.log(abc123);
  ```

- 不能以数字开头

  ```javascript
  var 123abc = 888; //报错
  console.log(123abc);
  ```

- 严格区分大小写，比如test和Test是2个不同的标识符

  ```javascript
  /*涨停, 张婷*/
  var test = 123;
  var tEst = 321;
  console.log(test);
  console.log(tEst);
  ```

- 不可以使用关键字、保留字作为标识符

  ```javascript
  let fjdsklfjslk = "lnj";
  let name = "lnj"; //报错 name在javascript中是关键字
  let lastname = "Lee";
  let lastName = "Lee";
  ```

- 可以使用中文,但是没人会这么去干!

  ```javascript
  let 哈哈_text = "abc";
  console.log(哈哈_text);
  ```

  

#### 2.1.7 注释

**什么是JS的注释?** 和HTML/CSS的注释一样, 都是用来注解解释某一段程序的含义的, 都是用来提升代码的阅读性的, 都是为了方便程序员之间沟通的

**JS注释的格式**

- 单行注释: `// 被注释的内容`

  注意点: 单行注释的有效范围是从第二个斜杠开始一直直到这一行的末尾, 也就是被注释的内容不能换行

- 多行注释: `/* 被注释的内容 */` 

  多行注释的有效范围是从第一颗`*`开始直到第二颗`*`结束, 也就是说被注释的内容可以换行的

**JS中注释的嵌套规则**

- 单行注释可以嵌套单行注释, 但是必须在一行

  ```javascript
  // 被注释的内容   // 被注释的内容
  ```

- 单行注释可以嵌套多行注释, 但是必须在一行

  ```javascript
   // 被注释的内容 /* 被注释的内容 */
  ```

- 多行注释可以嵌套单行注释

  ```javascript
  /*
  被注释的内容
  // 被注释的内容
  */
  ```

- 多行注释不可以嵌套多行注释

  ```javascript
  /*
  被注释的内容
  /*
  */
  */
  ```

  

### 2.2 数据类型

#### typeof

在JavaScript中为了方便我们检测某一种数据是属于哪一种数据类型的, JavaScript提供了一个名称叫做typeof的操作符

格式: `typeof 需要检测的数据;`

#### 基本数据类型

*javascript中的基本数据类型有5种: Number,String,Boolean,Undefined,NUll*

**Number 数值类型** 在JavaScript中无论是整数还是小数都是属于数值类型的

```javascript
let num = 42;
let num1 = 21.2;

// 利用typeof检测123这个常量是属于哪一种数据类型的, 并且将检测的结果保存到res这个变量中
// let res = typeof 123;
let res = typeof 3.14;
console.log(res);
```

**String 字符串类型** 在JavaScript中无论是通过单引号还是通过双引号括起来的内容都是属于字符串类型的

```javascript
let str = "hello";
let str1 = 'hello';

// let res = typeof 'abc';
let res = typeof "def";
console.log(res);
```

**Boolean 布尔类型** 在JavaScript中布尔类型比较特殊, 只有两个取值true/false

```javascript
let flag = true;
let falg1 = false;

// let res = typeof true;
let res = typeof false;
console.log(res);
```

**Undefined 未定义类型** 在JavaScript中未定义类型比较特殊, 只有一个取值undefined

```javascript
let num;
// 以下代码的含义是输出num变量中保存的数据
// 由于没有给num这个变量进行初始化, 所以这个变量中保存的数据默认是undefined
console.log(num);
// 利用typeof检查num中保存的数据是什么类型的
// 也就是说利用typeof检查undefined是属于什么类型的
let res = typeof num;
console.log(res);
```

**Null 空类型**

```javascript
let n = null;
```



#### 引用数据类型

Object 对象类型

#### 数据类型和常量的关系

猫科动物和狮子老虎豹子的关系

- 今天学习的数据类型就是猫科动物
- 前面学习的常量就是狮子老虎豹子



### 2.3 数据类型转换

javascript中的数据类型可以通过不同的手段来进行相互转换的

#### 转换为字符串类型

将`Number,Boolean,undefined,null`类型转换为字符串类型
在JavaScript中如果想将以上的四种基本数据类型转换为字符串类型, 常用的方法有三种

1. 对于Number类型和Boolean类型来说, 可以通过 变量名称.toString()的方式来转换

   ```javascript
   let value = 123;
   console.log(value); // 在谷歌浏览器的控制台中如果是Number类型是蓝色的
   console.log(typeof value);
   // 以下代码的含义: 将value变量中存储的数据拷贝一份, 然后将拷贝的数据转换为字符串之后返回
   let str = value.toString();
   console.log(str); // 在谷歌浏览器的控制台中如果是String类型是灰色的
   console.log(typeof str);
   // 注意点: 变量名称.toString是对拷贝的数据进行转换, 所以不会影响到原有的数据
   console.log(value);
   console.log(typeof value);
   // 注意点: 不能使用常量直接调用toString方法, 因为常量是不能改变的
   let str2 = 123.toString();
   ```

   ```javascript
   let value = true;
   console.log(value);
   console.log(typeof value);
   let str = value.toString();
   console.log(str);
   console.log(typeof str);
   ```

2. 可以通过String(常量or变量);转换为字符串

   ```javascript
   let value = undefined;
   console.log(value);
   console.log(typeof value);
   // 以下代码的含义: 根据传入的数据重新生成一个新的字符串
   let str = String(value);
   console.log(str);
   console.log(typeof str);
   ```

   ```javascript
   let value = null;
   console.log(value);
   console.log(typeof value);
   let str = String(value);
   console.log(str);
   console.log(typeof str);
   ```

   ```javascript
   let value = 123;
   console.log(value);
   console.log(typeof value);
   let str = String(value);
   console.log(str);
   console.log(typeof str);
   ```

   ```javascript
   // 注意点: 变量名称.toString()的方式前面不能是常量, 因为常量不能被改变
   //          String(常量or变量), 因为是根据传入的值重新生成一个新的值, 并不是修改原有的值
   let str = String(123);
   console.log(str);
   console.log(typeof str);
   ```

   ```javascript
   let value = true;
   console.log(value);
   console.log(typeof value);
   let str = String(value);
   console.log(str);
   console.log(typeof str);
   ```

   

3. 还可以通过 变量or常量 + `"" `/ 变量or常量 +` ''`转换为字符串

   ```javascript
   let value = 123;
   // let str = value + '';
   let str = value + "";
   console.log(str);
   console.log(typeof str);
   ```

   ```javascript
   let str = 123 + ''; // +''或者+""底层的本质其实就是调用String()函数
   console.log(str);
   console.log(typeof str);
   ```

   

#### 转换为数值类型 🚩



#### 转换为布尔类型

### 2.4 运算符

#### 算数运算符

#### 赋值运算符

#### 自增自减运算符

#### 关系运算符

#### 逻辑运算符

#### 逗号运算符

#### 三目运算符



### 2.5 流程控制

#### 5.1 顺序结构



#### 5.2 选择结构

##### 5.2.1 if

##### 5.2.2 switch

#### 5.3 循环结构

##### 5.3.1 while

##### 5.3.2 dowhile

##### 5.3.3 for



### 2.6 数组

#### 6.1 数组基本使用

#### 6.2 数组遍历

#### 6.3 数组解构赋值

#### 6.4 数组增删改查

#### 6.5 数组常用方法

#### 6.6 二维数组



### 2.7 函数

#### 2.7.1 函数基本使用

#### 2.7.2 函数argments

#### 2.7.3 函数扩展运算符

#### 2.7.4 函数形参默认值

#### 2.7.5 函数作为参数

#### 2.7.6 函数作为返回值

#### 2.7.7 匿名函数

#### 2.7.8 箭头函数

#### 2.7.9 递归函数

#### 2.7.10 函数中变量作用域

#### 2.7.11 作用域链

#### 2.7.12 预解析



### 2.8 面向对象





## 第 3 章: DOM及特效

### 3.1 DOM开篇

## 第 4 章: BOM及特效

### 4.1 BOM开篇



