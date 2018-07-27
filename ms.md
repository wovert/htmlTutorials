# 前段面试

## 开放性题目（15分钟）

1. 为什么离职
2. 你为什么加入我们公司
3. 你在现在的团队处于什么样的角色，起到了什么明显的作用？
4. 你在开发中遇到什么难题，怎么解决的？
5. 说说前端最近流行些什么，在自己以前的项目中都有应用哪些？常去哪些网站？
6. 对公司加班问题，你怎么样？
7. 公司出差？

## 技术型题目

### 你对分类信息网有什么看法和建议？(1分钟)

### 访问网站的流程是什么？（5分钟）

### HTTP协议有哪些响应类代码，分别代表什么含义？（1分钟）

### 微信公众号和小程序是怎么回事？（1分钟）

### 项目中使用哪些设计模式？用在哪些具体方面

### 数据结构与算法了解多少? 项目中用在哪些地方？

### 请写出目前有哪些主流浏览器及对应的内核是叫什么？（1分钟）

1. IE浏览器内核：Trident内核，也被称为IE内核；
2. Chrome浏览器内核：Chromium内核 → Webkit内核 → Blink内核；
3. Firefox浏览器内核：Gecko内核，也被称Firefox内核；
4. Safari浏览器内核：Webkit内核；
5. Opera浏览器内核：最初是自主研发的Presto内核，后跟随谷歌，从Webkit到Blink内核；
6. 360浏览器、猎豹浏览器内核：IE+Chrome双内核；
7. 搜狗、遨游、QQ浏览器内核：Trident（兼容模式）+Webkit（高速模式）；
8. 百度浏览器、世界之窗内核：IE内核；

### 请解释下JavaScript的同源策略？(1分钟)

同源策略简单的说就是一段脚本只能读取来自于同一来源的窗口和文档的属性，这里的同一来源指的是主机名、协议和端口号的组合。

### 什么是跨域，跨域请求资源的方法有哪些，你是如何解决跨域的？（3分钟）

由于浏览器同源策略，凡是发送请求url的协议、域名、端口三者之间任意一与当前页面地址不同即为跨域。存在跨域的情况：

网络协议不同，如http协议访问https协议。

端口不同，如80端口访问8080端口。

域名不同，如qianduanblog.com访问baidu.com。

子域名不同，如abc.qianduanblog.com访问def.qianduanblog.com。

域名和域名对应ip,如www.a.com访问20.205.28.90.

### 跨域请求资源的方法（5分钟）

1. porxy代理

> 定义和用法：proxy代理用于将请求发送给后台服务器，通过服务器来发送请求，然后将请求的结果传递给前端。

实现方法：通过nginx代理；
注意点：1、如果你代理的是https协议的请求，那么你的proxy首先需要信任该证书（尤其是自定义证书）或者忽略证书检查，否则你的请求无法成功。

2. CORS 【Cross-Origin Resource Sharing】

> 定义和用法：是现代浏览器支持跨域资源请求的一种最常用的方式。

使用方法：一般需要后端人员在处理请求数据的时候，添加允许跨域的相关操作。如下：
res.writeHead(200, {
  "Content-Type": "text/html; charset=UTF-8",
  "Access-Control-Allow-Origin":'http://localhost',
  'Access-Control-Allow-Methods': 'GET, POST, OPTIONS',
  'Access-Control-Allow-Headers': 'X-Requested-With, Content-Type'
});

3. jsonp

> 定义和用法：通过动态插入一个script标签。浏览器对script的资源引用没有同源限制，同时资源加载到页面后会立即执行（没有阻塞的情况下）。

特点：通过情况下，通过动态创建script来读取他域的动态资源，获取的数据一般为json格式。

html：

``` html: 原生js  
<html><head><title></title>  
  <script type="text/javascript">  

    var returnjs = function(data){  
        alert(data.code);  
    };  
    // 提供jsonp服务的url地址（不管是什么类型的地址，最终生成的返回值都是一段javascript代码）  
    var url = "http://www.return.com/jsonp/get?code=1&callback=returnjs";//数据接收后台  
    // 创建script标签，设置其属性  
    var script = document.createElement('script');  
    script.setAttribute('src', url);  
    // 把script标签加入head，此时调用开始  
    document.getElementsByTagName('head')[0].appendChild(script);  
    </script>  
</head><body></body></html>
```

``` html:jQ版
<!DOCTYPE html><html><head><meta charset="UTF-8"><title>Insert title here1</title>  
<script type="text/javascript" src="jq.js"></script><!-- 记得引入jq -->  
</head><body>  
<script type="text/javascript">
  jQuery(document).ready(function(){  
    $.ajax({  
      type: "get",  
      async: false,  
      url: "http://www.return.com/jsonp/get?code=1",//数据接收后台  
      dataType: "jsonp",  
      jsonp: "callback",//传递给请求处理程序或页面的，用以获得jsonp回调函数名的参数名(一般默认为:callback)  
      jsonpCallback:"returnjs",//自定义的jsonp回调函数名称，默认为jQuery自动生成的随机函数名，也可以写"?"，jQuery会自动为你处理数据  
      crossDomain:true,  
      success: function(json){  
          alert(json.code);
      },  
      error: function(){  
          alert('fail');  
      }  
    });  
  });  
</script></body></html>
```

### 缺点

1. 这种方式无法发送post请求（这里）
2. 另外要确定jsonp的请求是否失败并不容易，大多数框架的实现都是结合超时时间来判定。

## 请说说get和post请求的区别，什么时候用post？HEAD,PUT,DELETE

1. GET：一般用于信息获取，使用URL传递参数，对所发送信息的数量也有限制，一般在2000个字符，安全性很低
2. POST：一般用于修改服务器上的资源，通过提交表单来传值，对所发送的信息没有限制，安全性比GET高
3. 在以下情况中，使用 POST 请求：

无法使用缓存文件（更新服务器上的文件或数据库）

向服务器发送大量数据（POST 没有数据量限制）

发送包含未知字符的用户输入时，POST 比 GET 更稳定也更可靠

需要修改服务器上的资源的时候

### 你有哪些性能优化的方法？(5分钟)

1. 减少 HTTP 请求
2. 减少 DNS 查找
3. 避免重定向
4. 使用 Ajax 缓存
5. 延迟载入组件
6. 预先载入组件
7. 减少 DOM 元素数量
8. 切分组件到多个域
9. 最小化 iframe 的数量
10. 不要出现http 404 错误

### 请写出函数运行结果

``` js
function test(){
  var a;
  function foo(){
    return 2;
  }
  console.log(a);
  console.log(foo());
  a = 1;
}
test();
```

### 下面程序的结果

``` js
function fun(n,o){
  console.log(o);
  return {
    fun:function(m){
      return fun(m,n);
    }
  }
}
var a = fun(0);a.fun(1);a.fun(2);a.fun(3);
var b = fun(0).fun(1).fun(2).fun(3);
var c = fun(0).fun(1);c.fun(2);c.fun(3);
```

//undefined 1 1 1
//undefined 1 2 3
//undefined 1 2 2

### 下面输出的结果

``` js
var a = 9;
var b = a++ + a-- + ++a + --a + a--
```

``` js
//console.log(a);//0
//console.log(b);//48
```

### 什么是盒子模型？

> 一般来说，css盒子模型有两种模式：

W3C的标准模型 相当于 box-sizing：content-box 

我们对元素设置的宽度和高度就是内容（content）的宽度和高度，也就是内盒子的宽度；外盒子的宽度包括：content+padding+border的。

当我们设置好了宽度和高度的时候，其外盒子的宽度和高度基本上就定了，当我们想在内容（content）外面设置padding和margin或者border时，非常容易破坏我们的布局，此时我们就需要想到box-sizing：border-box

IE的传统模型 相当于box-sizing：border-box

这个模型下，我们设置的宽度和高度是包括：content+padding+border的，但是不包括margin。其内容的宽度比我们设置的宽度要小的。

如果计算的content的内容宽度为负值，其都会被计算为0，内容还在，故不能通过border-box来隐藏元素。元素的内容宽度和高度是在我们设置的宽度和高度的里面渲染的。当我们想给元素添加border或者padding时，这个模型不会破坏我们的布局，因为其会适当的调整我们内容content的宽度和高度来满足。故可以用来设置自适应布局

### let，const，var 区别？

- var：函数作用域，存在变量提升
- let：块作用域，不存在变量提升
- const：不能修改的是栈内存在的值和地址。声明一个基本类型的时候为常量，不可修改；声明对象可以修改

## 以下代码显示结果为

``` js
var objName = "name1";
function obj(){
    var objName = "name2";
    function innerObj(){
        alert(objName);
    }
    innerObj();
}
console.log(obj());
```

``` js
//name2
//undefined
```

## 求数组最大值

1. sort排序（先把数组从小到大排序，数组第一个即为最小值，最后一个即为最大值）

``` js
ary.sort(function(a,b){return a-b;});
var minN = ary[0];
var maxN = ary[ary.length-1];
```

2. 假设数组第一个为最大（或最小值），和后边进行比较，若后边的值比最大值大（或比最小值小），则替换最大值（或最小值）

``` js
var maxN = ary[0];
var minN = ary[0];
for(var i=1;i<ary.length;i++){
    var cur = ary[i];
    cur>maxN ? maxN=cur : null;
    cur<minN ? minN=cur : null;
}
```

3. Math的max和min方法

``` js
Math.max.apply(null, a);
Math.min.apply(null, a);
```

call()方法和apply()方法用法总结

## 编写一个方法 去掉一个数组的重复元素

方法一

``` js
var arr = [0,2,3,4,4,0,2];
var obj = {};
var tmp = [];
for(var i = 0 ;i< arr.length;i++){
   if( !obj[arr[i]] ){
      obj[arr[i]] = 1;
      tmp.push(arr[i]);
   }
}
console.log(tmp);
```

结果如下： [0, 2, 3, 4]

方法二：
```
var arr = [2,3,4,4,5,2,3,6],
   arr2 = [];
for(var i = 0;i<arr.length;i++){
    if(arr2.indexOf(arr[i]) < 0){
        arr2.push(arr[i]);
    }
}
console.log(arr2);
```

结果为：[2, 3, 4, 5, 6]

方法三：

``` js
var arr = [2,3,4,4,5,2,3,6];
var arr2 = arr.filter(function(element,index,self){
    return self.indexOf(element) === index;
});
console.log(arr2);
```

结果为：[2, 3, 4, 5, 6]

方法三中用到的函数说明：

filter方法用于过滤数组成员，满足条件的成员组成一个新数组返回。

它的参数是一个函数，所有数组成员依次执行该函数，返回结果为true的成员组成一个新数组返回。该方法不会改变原数组。

filter方法的参数函数可以接受三个参数：当前成员，当前位置和整个数组。

indexOf方法返回给定元素在数组中第一次出现的位置，如果没有出现则返回-1。

### 用css实现垂直居中

CSS垂直居中的11种实现方式

1. 使用绝对定位和负外边距对块级元素进行垂直居中

html代码：
``` html
<div id="box">
    <div id="child">我是测试DIV</div>
</div>
css代码：

复制代码
#box {
    width: 300px;
    height: 300px;
    background: #ddd;
    position: relative;
}
#child {
    width: 150px;
    height: 100px;
    background: orange;
    position: absolute;
    top: 50%;
    margin: -50px 0 0 0;
    line-height: 100px;
}
```

这个方法兼容性不错，但是有一个小缺点：必须提前知道被居中块级元素的尺寸，否则无法准确实现垂直居中。
 
2. 使用绝对定位和transform


``` html代码
<div id="child">
    我是一串很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长的文本
</div>

css代码：

#box {
    width: 300px;
    height: 300px;
    background: #ddd;
    position: relative;
}
#child {
    background: #93BC49;
    position: absolute;
    top: 50%;
    transform: translate(0, -50%);
}
```

这种方法有一个非常明显的好处就是不必提前知道被居中元素的尺寸了，因为transform中translate偏移的百分比就是相对于元素自身的尺寸而言的。

3. 另外一种使用绝对定位和负外边距进行垂直居中的方式


``` html代码：
<div id="box">
    <div id="child">我也是个测试DIV</div>
</div>
#box {
    width: 300px;
    height: 300px;
    background: #ddd;
    position: relative;
}
#child {
　　width: 50%;
    height: 30%;
    background: pink;
    position: absolute;
    top: 50%;
    margin: -15% 0 0 0;
}
复制代码
运行结果如下：



　　这种方式的原理实质上和前两种相同。补充的一点是：margin的取值也可以是百分比，这时这个值规定了该元素基于父元素尺寸的百分比，可以根据实际的使用场景来决定是用具体的数值还是用百分比。
```

``` 绝对定位结合margin: auto
html代码
<div id="box">
    <div id="child">呆呆今天退役了(。﹏。)</div>
</div>
#box {
    width: 300px;
    height: 300px;
    background: #ddd;
    position: relative;
}
#child {
    width: 200px;
    height: 100px;
    background: #A1CCFE;
    position: absolute;
    top: 0;
    bottom: 0;
    margin: auto;
    line-height: 100px;
}
```

这种实现方式的两个核心是：把要垂直居中的元素相对于父元素绝对定位，top和bottom设为相等的值，我这里设成了0，当然你也可以设为99999px或者-99999px无论什么，只要两者相等就行，这一步做完之后再将要居中元素的margin设为auto，这样便可以实现垂直居中了。

被居中元素的宽高也可以不设置，但不设置的话就必须是图片这种自身就包含尺寸的元素，否则无法实现。
 
5. 使用padding实现子元素的垂直居中

``` html
```
<div id="box">
    <div id="child">今天西安的霾严重的吓人，刚看了一眼PM2.5是422</div>
</div>
#box {
    width: 300px;
    background: #ddd;
    padding: 100px 0;
}
#child {
    width: 200px;
    height: 100px;
    background: #F7A750;
    line-height: 50px;
}
```

这种实现方式非常简单，就是给父元素设置相等的上下内边距，则子元素自然是垂直居中的，当然这时候父元素是不能设置高度的，要让它自动被填充起来，除非设置了一个正好等于上内边距+子元素高度+下内边距的值，否则无法精确的垂直居中。
这种方式看似没有什么技术含量，但其实在某些场景下也是非常好用的。
 
6. 设置第三方基准

``` html
html代码：
<div id="box">
    <div id="base"></div>
    <div id="child">今天写了第一篇博客，希望可以坚持写下去！</div>
</div>
#box {
    width: 300px;
    height: 300px;
    background: #ddd;
}
#base {
    height: 50%;
    background: #AF9BD3;
}
#child {
    height: 100px;
    background: rgba(131, 224, 245, 0.6);
    line-height: 50px;
    margin-top: -50px;
}
```

这种方式也非常简单，首先设置一个高度等于父元素高度一半的第三方基准元素，那么此时该基准元素的底边线自然就是父元素纵向上的中分线，做完这些之后再给要垂直居中的元素设置一个margin-top，值的大小是它自身高度的一半取负，则实现垂直居中。
 
7. 使用flex布局

```
<div id="box">雾霾天气，太久没有打球了</div>
```

#box {
    width: 300px;
    height: 300px;
    background: #ddd;
    display: flex;
    align-items: center;
}
```

这种方式同样适用于块级元素：

``` html
<div id="box">
    <div id="child">
        程序员怎么才能保护好眼睛？
    </div>
</div>
#box {
    width: 300px;
    height: 300px;
    background: #ddd;
    display: flex;
    align-items: center;
}
#child {
    width: 300px;
    height: 100px;
    background: #8194AA;
    line-height: 100px;
}
```

flex布局（弹性布局/伸缩布局）里门道颇多，这里先针对用到的东西简单说一下，想深入学习的小伙伴可以去看阮一峰老师的博客。

flex也就是flexible，意为灵活的、柔韧的、易弯曲的。

元素可以通过设置display:flex;将其指定为flex布局的容器，指定好了容器之后再为其添加align-items属性，该属性定义项目在交叉轴（这里是纵向轴）上的对齐方式，可能的取值有五个，分别如下：

- flex-start:：交叉轴的起点对齐；
- flex-end：交叉轴的终点对齐；
- center：交叉轴的中点对齐；
- baseline：项目第一行文字的基线对齐；
- stretch（该值是默认值）：如果项目没有设置高度或者设为了auto，那么将占满整个容器的高度。

8. 第二种使用弹性布局的方式

``` html
<div id="box">
    <div id="child">
        答案当然是多用绿色的背景哈哈
    </div>
</div>
#box {
    width: 300px;
    height: 300px;
    background: #ddd;
    display: flex;
    flex-direction: column;
    justify-content: center;
}
#child {
    width: 300px;
    height: 100px;
    background: #08BC67;
    line-height: 100px;
}
```

这种方式也是首先给父元素设置display:flex，设置好之后改变主轴的方向flex-direction: column，该属性可能的取值有四个，分别如下：

row（该值为默认值）：主轴为水平方向，起点在左端；
row-reverse：主轴为水平方向，起点在右端；
column：主轴为垂直方向，起点在上沿；
column-reverse：主轴为垂直方向，起点在下沿。
　　
justify-content属性定义了项目在主轴上的对齐方式，可能的取值有五个，分别如下（不过具体的对齐方式与主轴的方向有关，以下的值都是假设主轴为从左到右的）：
flex-start（该值是默认值）：左对齐；
flex-end：右对齐；
center：居中对齐；
space-between：两端对齐，各个项目之间的间隔均相等；
space-around：各个项目两侧的间隔相等。


9. 还有一种在前面已经见到过很多次的方式就是使用 line-height 对单行文本进行垂直居中

```
<div id="box">
    我是一段测试文本
</div>
#box{
    width: 300px;
    height: 300px;
    background: #ddd;
    line-height: 300px;
}
```
这里有一个小坑需要大家注意：line-height(行高) 的值不能设为100%，我们来看看官方文档中给出的关于line-height取值为百分比时候的描述：基于当前字体尺寸的百分比行间距。所以大家就明白了，这里的百分比并不是相对于父元素尺寸而言，而是相对于字体尺寸来讲的。
 
10. 使用 line-height 和 vertical-align 对图片进行垂直居中

``` html
<div id="box">
    <img src="duncan.jpeg">
</div>
#box{
    width: 300px;
    height: 300px;
    background: #ddd;
    line-height: 300px;
}
#box img {
    vertical-align: middle;
}
```

vertical-align并不像看起来那样天真无邪童叟无欺，以后会单独拎出来专门写一篇

11. 使用 display 和 vertical-align 对容器里的文字进行垂直居中

``` HTML
<div id="box">
    <div id="child">我也是一段测试文本</div>
</div>
#box {
    width: 300px;
    height: 300px;
    background: #ddd;
    display: table;
}
#child {
    display: table-cell;
    vertical-align: middle;
}
```

这里关于vertical-align啰嗦两句：vertical-align属性只对拥有valign特性的html元素起作用，例如表格元素中的<td><th>等等，而像<div><span>这样的元素是不行的。

valign属性规定单元格中内容的垂直排列方式，语法：<td valign="value">，value的可能取值有四种：

top：对内容进行上对齐
middle：对内容进行居中对齐
bottom：对内容进行下对齐
baseline：基线对齐

关于baseline值：基线是一条虚构的线。在一行文本中，大多数字母以基线为基准。baseline 值设置行中的所有表格数据都分享相同的基线。该值的效果常常与 bottom 值相同。不过，如果文本的字号各不相同，那么 baseline 的效果会更好。

## 用es6的方法将数组[5,6,7]插入数组[1,2,3,4]，得到数组[1,2,5,6,7,3,4]。

``` js
var arry1 = [1,2,3,4];
var arry2 = [5,6,7];
//这里的...为扩展运算符（spread）：表示将一个数组转为用逗号分隔的参数序列。
arry1.splice(2,0,...arry2);
console.log(arry1);
```

splice方法用于删除原数组的一部分成员，并可以在删除的位置添加新的数组成员，返回值是被删除的元素。该方法会改变原数组。 

splice方法的第一个参数是删除的起始位置（从0开始），第二个参数是被删除的元素个数（第二个参数设为0的时候表示只插入元素）。如果后面还有更多的
参数，则表示这些就是要被插入数组的新元素。 

…扩展运算符（spread）：将一个数组转为用逗号分隔的参数序列。

## javascript 的typeof 返回哪些数据类型

> 共有7种类型 : undefined， string，boolean， number， symbol(ES6)，object ，function

``` js
var un;
var str = 'strings';
var value = true;
var num = 1;
var sym = Symbol();
var obj = {};
var arr = [];
var getName = function () {
    return this.name;
}
function setName(name) {
    this.name = name;
}
console.log(typeof un);//undefined
console.log(typeof str);//string
console.log(typeof value);//boolean
console.log(typeof num);//number
console.log(typeof sym);//symbol
console.log(typeof obj);//object
console.log(typeof arr);//object
console.log(typeof getName);//function
console.log(typeof setName);//function
```

- 事件绑定和普通事件有什么区别
- 事件绑定：

``` js
var btn = document.getElementById("demo1");
btn.addEventListener("click",function(){
    alert(1);
},false);
btn.addEventListener("click",function(){
    alert(2);
},false);
```

执行上面的代码会先弹出 1 再弹出2

普通事件：
``` js
var btn = document.getElementById("demo2");
btn.onclick = function(){
    alert(1);
}
btn.onclick = function(){
    alert(2);
}
```

执行上面的代码只会弹出2

区别：

普通添加事件的方法不支持添加多个事件，最下面的事件会覆盖上面的，而事件绑定（addEventListener）方式添加事件可以添加多个。
事件绑定是指把事件注册到具体的元素之上，普通事件指的是可以用来注册的事件
如何显示/隐藏一个dom 元素？请用原生的JavaScript 方法实现

display:none;

visibility:hidden;

定位的元素的 z-index 属性；

透明度: opacity:0 || rgba(0, 0, 0, 0) || color: transparent;

用正则表达式，写出由字母开头，其余由数字、字母、下划线组成的6~30的字符串？

^[a-zA-Z]{1}[/w]{5,29}

写一个函数可以计算sum(5,0,-5);输出0; sum(1,2,3,4);输出10;

``` js
function sum() {
    var result = 0;
    for (var value of arguments) {
        result += value;
    }
    return result;
}
console.log(sum(5,0,-5));//0
console.log(sum(1,2,3,4));//10
```

用正则写出正确的正则表达式匹配固话号，区号3-4 位，第一位为0，中横线，7-8 位数字，中横线，3-4 位分机号格式的固话号

/0[0-9]{2,3}-\d{7,8}/

请写一个正则表达式：要求最短6 位数，最长20 位，阿拉伯数和英文字母（不区分大小写）组成

/^[A-z0-9]{6,20}$/i

下列JavaScript 代码执行后，iNum 的值是

``` js
var a;
var b = a * 0;
if (b == b) {
    console.log(b * 2 + "2" - 0 + 4);
} else {
    console.log(!b * 2 + "2" - 0 + 4);
}
console.log(b * 2 + "2" - 0 + 4);//NAN
console.log('b的值是：'+b);//NAN
console.log('!b的值是：'+!b);//true
```

答案：26

``` js
<script>
var a = 1;
</script>
<script>
var a;
var b = a * 0;
if (b == b) {
    console.log(b * 2 + "2" - 0 + 4);
} else {
    console.log(!b * 2 + "2" - 0 + 4);
}
</script>
```

答案：6

``` js
var t = 10;
function test(t){
    var t = t++;
}
test(t);
console.log(t);
```

答案：10

``` js
var t = 10;
function test(test){
    var t = test++;
}
test(t);
console.log(t);
```


答案：10

``` js
var t = 10;
function test(test){
    t = t + test;
    console.log(t);
    var t = 3;
}
test(t);
console.log(t);
```
答案：NaN 10

``` js
var a;
var b = a / 0;
if (b == b) {
    console.log(b * 2 + "2" - 0 + 4);
} else {
    console.log(!b * 2 + "2" - 0 + 4);
}
console.log(b);//NAN
```

答案：26

```
<script>
    var a = 1;
</script>
<script>
    var a;
    var b = a / 0;
    if (b == b) {
        console.log(b * 2 + "2" + 4);
    } else {
        console.log(!b * 2 + "2" + 4);
    }
    console.log(b);//Infinity
    console.log(b==b);//true
    console.log(!b * 2 + "2" + 4);//024
    console.log(!b);//false
</script>
```

答案：Infinity24

## 下列JavaScript 代码执行后，运行的结果是
```
<button id='btn'>点击我</button>
var btn = document.getElementById('btn');
var handler = {
        id: '_eventHandler',
        exec: function(){
            alert(this.id);
        }
}
btn.addEventListener('click', handler.exec,false);
```

答案： btn

## 下列JavaScript 代码执行后，依次输出的结果

``` js
var obj = {proto: {a:1,b:2}};
function F(){};
F.prototype = obj.proto;
var f = new F();
obj.proto.c = 3;
obj.proto = {a:-1, b:-2};
console.log(f.a);//1
console.log(f.c);//3
delete F.prototype['a'];
console.log(f.a);//undefined
console.log(obj.proto.a);//-1
```

下列JavaScript 代码执行后的li 元素的数量是
```
<ul>
<li>Item</li>
<li></li>
<li></li>
<li>Item</li>
<li>Item</li>
</ul>
var items = document.getElementsByTagName('li');
    for(var i = 0; i< items.length; i++){
        if(items[i].innerHTML == ''){
            items[i].parentNode.removeChild(items[i]);
        }
}
```

答案：这里写图片描述

正则表达式构造函数var reg=new RegExp(“xxx”)与正则表达字面量varreg=//有什么不同？匹配邮箱的正则表达式？
答案：

当使用RegExp()构造函数的时候，不仅需要转义引号（即\”表示”），并且还需要双反斜杠（即\表示一个\）。
使用正则表达字面量的效率更高。
用js 实现随机选取10–100 之间的10 个数字，存入一个数组，并排序。
``` js
var iArray = [];
function getRandom(istart, iend) {
    var iChoice = iend - istart + 1;
    return Math.floor(Math.random() * iChoice) + istart;
}
for (var i = 0; i < 10; i++) {
    iArray.push(getRandom(10, 20));
}
iArray.sort();
console.log(iArray.toString())
```

写出答案

``` js
var a=1;
console.log(a++); //答案：1
console.log(++a); //答案：3
console.log(null==undefined); //答案：true
console.log(“1”==1); //答案：true，因为会将数字1 先转换为字符串1
console.log(“1”===1); //答案：false，因为数据类型不一致
typeof 1; “number”
typeof “hello”; “string”
typeof /[0-9]/; “object”
typeof {}; “object”
typeof null; “object”
typeof undefined; “undefined”
typeof [1,2,3]; “object”
typeof function(){}; //”function”
parseInt(3.14); //3
parseFloat(“3asdf”); //3
parseInt(“1.23abc456”);//1
parseInt(true);//”true” NaN

parseInt(string, radix)
```

- 注意： 只有字符串中的第一个数字会被返回。 
- 注意： 开头和结尾的空格是允许的。 
- 注意：如果字符串的第一个字符不能被转换为数字，那么 parseInt() 会返回 NaN。

结果
//考点：函数声明提前

```
function bar() {
    return foo;
    foo = 10;
    function foo() {}
    //var foo = 11;
}
alert(typeof bar());//"function"

var foo = 1;
function bar() {
    foo = 10;
    return;
    function foo() {}
}
bar();
alert(foo);//1

console.log(a);//是一个函数
var a = 3;
function a(){}
console.log(a);//3

bar();//bar is not defined
var foo = function bar(name) {
    console.log("hello"+name);
    console.log(bar);
};
console.log(typeof bar)//undefined
foo("world");//helloworld [Function: bar]
console.log(bar);//bar is not defined
console.log(foo.toString());/*function bar(name) {
    console.log("hello"+name);
    console.log(bar);
}
```

## 正则表达式对象 清除空格

写一个function，清除字符串前后的空格。（兼容所有浏览器） 

使用自带接口trim()，考虑兼容性：

``` js
if (!String.prototype.trim) {
String.prototype.trim = function() {
    return this.replace(/^\s+/, "").replace(/\s+$/,"");
    } 
}
// test the function
var str = " \t\n test string ".trim();
alert(str == "test string"); // alerts "true"
```

## 程序中捕获异常的方法？

``` js
window.error
try{}catch(){}finally{}
```

## JavaScript 数组元素添加、删除、排序等方法有哪些？

```
Array.concat( ) 连接数组 
Array.join( ) 将数组元素连接起来以构建一个字符串 
Array.length 数组的大小 
Array.pop( ) 删除并返回数组的最后一个元素 
Array.push( ) 给数组添加元素 
Array.reverse( ) 颠倒数组中元素的顺序 
Array.shift( ) 将元素移出数组 
Array.slice( ) 返回数组的一部分 
Array.sort( ) 对数组元素进行排序 
Array.splice( ) 插入、删除或替换数组的元素 
Array.toLocaleString( ) 把数组转换成局部字符串 
Array.toString( ) 将数组转换成一个字符串 
Array.unshift( ) 在数组头部插入一个元素
```

## document load 和document ready 的区别

- Document.onload 是在结构和样式加载完才执行js
- Document.ready 原生种没有这个方法，jquery 中有$().ready(function)

## 阶乘函数

//原型方法
``` js
Number.prototype.N = function(){
    var re = 1;
    for(var i = 1; i <= this; i++){
        re *= i;
    }
    return re;
}
var num = 5;
alert(num.N());
```

## window.location.search() 返回的是什么？

答：查询(参数)部分。除了给动态语言赋值以外，我们同样可以给静态页面,并使用javascript 来获得相信应的参数值 
返回值：?ver=1.0&id=timlq 也就是问号后面的！

## window.location.hash 返回的是什么？

答：锚点， 返回值：#love ；

## window.location.reload() 作用？

答：刷新当前页面。

## 阻止冒泡兼容写法

``` js
function stopPropagation(e) {
    e = e || window.event;
    if(e.stopPropagation) { //W3C 阻止冒泡方法
        e.stopPropagation();
    } else {
    e.cancelBubble = true; //IE 阻止冒泡方法
    }
}
document.getElementById('need_hide').onclick = function(e) {
    stopPropagation(e);
}
```

## 什么是闭包？ 写一个简单的闭包？；

答：我的理解是，闭包就是能够读取其他函数内部变量的函数。在本质上，闭包就是将函数内部和函数外部连接起来的一座桥梁。

```
function outer(){
  var num = 1;
  function inner(){
    var n = 2;
    alert(n + num);
  }
  return inner;
}
outer()();
```

解释下
``` js
function changeObjectProperty (o) {
    o.siteUrl = "http://www.csser.com/";
    o = new Object();
    o.siteUrl = "http://www.popcg.com/";
}
var CSSer = new Object();
changeObjectProperty(CSSer);
console.log(CSSer.siteUrl);
```

如果CSSer 参数是按引用传递的，那么结果应该是"http://www.popcg.com/"，但实际结果却仍是"http://www.csser.com/"。 
事实是这样的：在函数内部修改了引用类型值的参数，该参数值的原始引用保持不变。我们可以把参数想象成局部变量，当参数被重写时，这个变量引用的就是一个局部变量，局部变量的生存期仅限于函数执行的过程中，函数执行完毕，局部变量即被销毁以释放内存。 
（补充：内部环境可以通过作用域链访问所有的外部环境中的变量对象，但外部环境无法访问内部环境。每个环境都可以向上搜索作用域链，以查询变量和函数名，反之向下则不能。）

阅读更多
个人分类： 前端面试题-JS
相关热词： with在js  与js  js的is  js的then

##  两个div标签，如何控制标签左边固定，右边自适应，左边div宽度为100px.

- 左边设置浮动，右边不设置宽度自定布局到右边

```
<!doctype html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
  <style>
    .container{
      width: 1000px;
      height: 500px;
      border: 1px solid #ccc;
    }
    .left{
      width: 100px;
      height: 100%;
      background: red;
      float: left;
    }
    .right{
      overflow: hidden;/*解决左右两个块之间的重叠的问题，或者用margin-left:100px;来解决*/
      background: green;
      height: 100%;
   }
  </style>
</head>
<body>
  <div class="container">
    <div class="left">我是左边,固定宽度</div>
    <div class="right">我是右边,自适应</div>
  </div>
</body>
</html>
```

这种方法是存在兼容问题的，如果只是在手机上用这种完全没有问题，但是如果在PC上会有兼容问题，在IE7下这种方法会出现问题，如果右边格子没有内容就不会出现问题，但是如果右边格子有内容并且是inline的，那么左边格子就会出现一条无法控制的分割线。

最好的解决办法就是左右都浮动，然后右边用负的margin-left把它拉回来，如果左右都浮动，左边100px,右边百分之百正常来说就是右边就被挤到了下面，所以用负的margin值把它拉回来，但是这样就会带来一个问题，左右会出现重叠的部分，解决办法就是在右边div里面再套一层，加一个正的margin-left,大小和左边相同，这种方法是所有的方法里面兼容性最好的，IE5都会支持，在没有table-cell和flex等方法的时候，流体布局都是用这种方式来做的。

具体代码如下：

``` js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=yes">
    <style>
        html, body {
            margin: 0;
            font-family: Helvetica;
            font-size: 16px;
        }
        #page {
            border-bottom: 1px solid #000;
            height: 100px;
        }
        #page .left,
        #page .right {
            float: left;
        }
        #page .left {
            width: 100px;
            background-color: #f00;
        }
        #page .right {
            width: 100%;
            margin-left: -100px;
        }
        #page .content {
            margin-left: 100px;/*解决左右出现重叠的情况*/
            background-color: #ccc;
            height: 50px;
        }
    </style>
</head>
<body>
<div id="page">
    <div class="left">左边</div>
    <div class="right">
        <div class="content">右边</div>
    </div>
</div>
</body>
</html>
```

(2)Flex布局

```
<!doctype html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		.container{
			width: 1000px;
			height: 500px;
			border: 1px solid #ccc;
			display: flex;
		}
		.left{
			width: 100px;
			height: 100%;
			background: red;
			flex:none;
		}
		.right{
			background: yellow;
			height: 100%;
			flex:1;
		}
	</style>
</head>
<body>
	<div class="container">
		<div class="left">我是左边,固定宽度</div>
		<div class="right">我是右边,自适应</div>
	</div>
</body>
</html>
```

这种方式是兼容性最差的，安卓5.0不支持，需要写前缀-webkit-box,ios8上也不支持。

(3)table布局

``` js
<!doctype html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
  <style>
    .container{
      width: 1000px;
			height: 500px;
			border: 1px solid #ccc;
			display: table;
		}
		.left{
			width: 100px;
			height: 100%;
			background: red;
			display: table-cell;/* 此元素会作为一个表格单元格显示（类似 <td> 和 <th>） */
		}
		.right{
			background: orange;
			height: 100%;
			display: table-cell;
		}
	</style>
</head>
<body>
	<div class="container">
		<div class="left">我是左边,固定宽度</div>
		<div class="right">我是右边,自适应</div>
	</div>
</body>
</html>
```

兼容性是IE10以上才完美的支持，IE678不支持，IE9支持的不好，但这种方式在手机上是最好的，最方便的流体布局方式

## (4)css动态计算宽度，clac

```
<!doctype html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		.container{
			width: 1000px;
			height: 500px;
			border: 1px solid #ccc;
			overflow: hidden; /* 清除浮动 */
		}
		.left{
			width: 100px;
			height: 100%;
			background: red;
			float: left;
		}
		.right{
			background: orange;
			height: 100%;
			width: calc(100% - 100px);
			float: right;
		}
	</style>
</head>
<body>
	<div class="container">
		<div class="left">我是左边,固定宽度</div>
		<div class="right">我是右边,自适应</div>
	</div>
</body>
</html>

```
注：calc用法



兼容性是IE9开始支持，必须加前缀，但也会有各种各样的bug,手机上市安卓4.0以上才支持，但是兼容也不好

（5）利用定位

```
<!doctype html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>Document</title>  
    <style>  
        .container{  
            width: 100%;  
            height: 500px;  
            border: 1px solid #ccc;  
            position:relative;  
        }  
        .left{  
            width: 100px;  
            height: 100%;  
            background: red;  
            position:absolute;  
            left:0;  
            top:0;  
        }  
        .right{  
            background: orange;  
            height: 100%;  
            position:absolute;  
            left:100px;  
            top:0px;  
            right:0px;  
        }  
    </style>  
</head>  
<body>  
    <div class="container">  
        <div class="left">我是左边,固定宽度</div>  
        <div class="right">我是右边,自适应</div>  
    </div>  
</body>  
</html>
```

其实定位就有两种方式可以实现，另一种是左边设置好宽度不定位，右边定位设置好left值，也可以实现就不写具体的代码了，这种方式的兼容性仅次于浮动，可以兼容到IE7以上的浏览器。

总结：从运算时间上来讲，能用盒模型解决的，绝不用float或者定位。手机上常用的布局方式是table-cell比较好。

## 浏览器地址栏输入一个url后，到页面展示出来，中间经历了什么？

①首先，在浏览器地址栏中输入url。

②浏览器先查看浏览器缓存-系统缓存-路由器缓存，如果缓存中有，会直接在屏幕中显示页面内容。若没有，则跳到第三步操作。

③在发送http请求前，需要域名解析(DNS解析)(DNS（域名系统，Domain Name System）是互联网的一项核心服务，它作为可以将域名和IP地址相互映射的一个分布式数据库，能够使人更方便的访问互联网，而不用去记住IP地址。)，解析获取相应的IP地址。

④浏览器向服务器发起tcp连接，与浏览器建立tcp三次握手。（TCP即传输控制协议。TCP连接是互联网连接协议集的一种。）

⑤握手成功后，浏览器向服务器发送http请求，请求数据包。

⑥服务器处理收到的请求，将数据返回至浏览器。

⑦浏览器收到HTTP响应。

⑧读取页面内容，浏览器渲染，解析html源码。

⑨生成Dom树、解析css样式、js交互

⑩客户端和服务器交互，ajax请求

## get和post的区别

GET和POST本质上就是TCP链接，并无差别。但是由于HTTP的规定和浏览器/服务器的限制，导致他们在应用过程中体现出一些不同。 GET和POST还有一个重大区别，简单的说：GET产生一个TCP数据包；POST产生两个TCP数据包。
对于GET方式的请求，浏览器会把http header和data一并发送出去，服务器响应200（返回数据）； 而对于POST，浏览器先发送header，服务器响应100 continue，浏览器再发送data，服务器响应200 ok（返回数据）。

GET后退按钮/刷新无害，POST数据会被重新提交（浏览器应该告知用户数据会被重新提交）。
GET书签可收藏，POST为书签不可收藏。
GET能被缓存，POST不能缓存 。
GET编码类型application/x-www-form-url，POST编码类型encodedapplication/x-www-form-urlencoded 或 multipart/form-data。为二进制数据使用多重编码。
GET历史参数保留在浏览器历史中。POST参数不会保存在浏览器历史中。
GET对数据长度有限制，当发送数据时，GET 方法向 URL 添加数据；URL 的长度是受限制的（URL 的最大长度是 2048 个字符）。POST无限制。
GET只允许 ASCII 字符。POST没有限制。也允许二进制数据。
与 POST 相比，GET 的安全性较差，因为所发送的数据是 URL 的一部分。在发送密码或其他敏感信息时绝不要使用 GET ！POST 比 GET 更安全，因为参数不会被保存在浏览器历史或 web 服务器日志中。
GET的数据在 URL 中对所有人都是可见的。POST的数据不会显示在 URL 中。

## 本地存储cookie,sessionstorage,localstorage

（1）由于HTTP协议是无状态的协议，所以服务端需要记录用户的状态时，就需要用某种机制来识具体的用户，这个机制就是Session.典型的场景比如购物车，当你点击下单按钮时，由于HTTP协议无状态，所以并不知道是哪个用户操作的，所以服务端要为特定的用户创建了特定的Session，用用于标识这个用户，并且跟踪用户，这样才知道购物车里面有几本书。这个Session是保存在服务端的，有一个唯一标识。在服务端保存Session的方法很多，内存、数据库、文件都有。集群的时候也要考虑Session的转移，在大型的网站，一般会有专门的Session服务器集群，用来保存用户会话，这个时候 Session 信息都是放在内存的，使用一些缓存服务比如Memcached之类的来放 Session。

（2）思考一下服务端如何识别特定的客户？这个时候Cookie就登场了。每次HTTP请求的时候，客户端都会发送相应的Cookie信息到服务端。实际上大多数的应用都是用 Cookie 来实现Session跟踪的，第一次创建Session的时候，服务端会在HTTP协议中告诉客户端，需要在 Cookie 里面记录一个Session ID，以后每次请求把这个会话ID发送到服务器，我就知道你是谁了。有人问，如果客户端的浏览器禁用了 Cookie 怎么办？一般这种情况下，会使用一种叫做URL重写的技术来进行会话跟踪，即每次HTTP交互，URL后面都会被附加上一个诸如 sid=xxxxx 这样的参数，服务端据此来识别用户。

（3）Cookie其实还可以用在一些方便用户的场景下，设想你某次登陆过一个网站，下次登录的时候不想再次输入账号了，怎么办？这个信息可以写到Cookie里面，访问网站的时候，网站页面的脚本可以读取这个信息，就自动帮你把用户名给填了，能够方便一下用户。这也是Cookie名称的由来，给用户的一点甜头。

Session是在服务端保存的一个数据结构，用来跟踪用户的状态，这个数据可以保存在集群、数据库、文件中；

Cookie是客户端保存用户信息的一种机制，用来记录用户的一些信息，也是实现Session的一种方式。
优缺点：

###（1）cookie

优点：

①极高的扩展性和可用性，通过良好的编程，控制保存在cookie中的session对象的大小

②通过加密和安全传输技术（SSL），减少cookie被破解的可能性

③控制cookie的生命期，使之不会永远有效。偷盗者很可能拿到一个过期的cookie。

缺点：

①Cookie数量和长度的限制。每个domain最多只能有20条cookie，每个cookie长度不能超过4KB，否则会被截掉

②安全性问题。如果cookie被人拦截了，那人就可以取得所有的session信息。即使加密也与事无补，因为拦截者并不需要知道cookie的意义，他只要原样转发cookie就可以达到目的了。

###（2）session

优点：

①如果要在诸多Web页间传递一个变量，那么用Session变量要比通过QueryString传递变量可使问题简化。

②你可以在任何想要使用的时候直接使用session变量，而不必事先声明它，这种方式接近于在VB中变量的使用。使用完毕后，也不必考虑将其释放，因为它将自动释放。

缺点：

①Session变量和cookies是同一类型的。如果某用户将浏览器设置为不兼容任何cookie，那么该用户就无法使用这个Session变量！
②当一个用户访问某页面时，每个Session变量的运行环境便自动生成，这些Session变量可在用户离开该页面后仍保留20分钟！（事实上，这些变量一直可保留至“timeout”。“timeout”的时间长短由Web服务器管理员设定。一些站点上的变量仅维持了3分钟，一些则为10分钟，还有一些则保留至默认值20分钟。）所以，如果在Session中置入了较大的对象那就有麻烦了！随着站点访问量的增大，服务器将会因此而无法正常运行！

③因为创建Session变量有很大的随意性，可随时调用，不需要开发者做精确地处理，所以，过度使用session变量将会导致代码不可读而且不好维护。