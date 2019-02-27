
#CSS优先级算法如何计算？
!important > 行内样式 > #id > .class > tag > * > 继承 > 默认

#盒模型
width+height => padding => border => margin
页面渲染时，dom 元素所采用的 布局模型。可通过box-sizing进行设置。根据计算宽高的区域可分为：
  content-box (W3C 标准盒模型) //测量width和height属性只包括的内容，不包括border, margin, 或者padding。
  border-box (IE 盒模型)  //width和height属性包括padding和border，不包括margin
  padding-box //width和height属性包括padding的大小，不包括border和margin
  inherit //指定box-sizing属性的值，应该从父元素继承

#flex布局
  * display: flex;
  * display: -webkit-flex;  /* Safari */
    * 主轴方向：flex-direction: row|row-reverse|column|column-reverse;（左到右|右到左|上到下|下到上）
    * 水平对齐：justify-content: flex-start|flex-end|center|space-between|space-around; (左对齐|右对齐|居中|2端对齐|中间对齐)
    * 垂直对齐： align-items: flex-start|flex-end|center|baseline|stretch; (上对齐|下对齐|垂直居中|项目第一行文字的基线对齐|占满)
    * 多根轴线对齐： align-conent: flex-start|flex-end|center|space-between|space-around|stretch;
    * 换行： flex-wrap: nowrap|wrap|wrap-reverse (不换行|换行第一行在上方|换行第一行在下方)
    * flex: 0 1 auto; (flex: flex-grow flex-shrink flex-basis)
    * flex-grow: 0; 等分剩余空间，0不等分，1按比例等分，如果一个为2，其他都为1，其占据的剩余空间是其他的2倍
    * flex-shrink: 1; 空间不足时项目缩小，0不缩小, 1按比例缩小
    * flex-basis: auto; 在分配多余空间前,计算主轴是否有多余空间，auto项目本来大小

# 水平垂直居中：
  * 未知宽高水平垂直居中
    <div class="parent">
      <div class="child">hello world</div>
    </div>
    方法一：父元素display:table; 子元素：display:table-cell，
            优势，父元素可以动态改变高度，劣势：table属性容易造成多次reflow,IE8以下不支持
        .parent{
          display: table;
        }
        .parent .child {
          display: table-cell;
          vertical-align: middle;
          text-align: center;
        }
    方法二：利用空元素或伪类，优势：兼容性好
        .parent {
          position: absolute;
          width: 100%;
          height: 100%;
          text-align: center;
          background: #92b922;
        }
        .child {
          background: #d93168;
          display: inline-block;
          color: #fff;
          padding: 20px;
        }
        .parent:after {
          display: inline-block;
          content: '';
          width: 0px;
          height: 100%;
          vertical-align: middle;
        }
    方法三：绝对定位+伪类, transform兼容性差，IE9以下不支持
        .parent{
          position:relative;
          width:300px;
          height:300px;
          background:#92b922;
        }
        .child{
          position:absolute;
          top:50%;
          left:50%;
          color:#fff;
          background:red;
          transform:translate(-50%,-50%);
        }
    方法四：flex布局，兼容性不好，IE支持性很差
      display: flex;
      align-items: center;
      justify-content: center;

# 优雅降级，渐进增强
渐进增强（Progressive Enhancement）：一开始就针对低版本浏览器进行构建页面，完成基本的功能，然后再针对高级浏览器进行效果、交互、追加功能达到更好的体验。

优雅降级（Graceful Degradation）：一开始就构建站点的完整功能，然后针对浏览器测试和修复。比如一开始使用 CSS3 的特性构建了一个应用，然后逐步针对各大浏览器进行 hack 使其可以在低版本浏览器上正常浏览。

#em, rem, px
  em:相对父元素  rem:相对根元素  px:像素

#rem.js
  function initRem(doc, win) {
    var html = document.getElementsByTagName("html")[0];
    var fs = parseInt(getStyle(html, 'fontSize'));

    function getStyle(obj, attr) {
      if (obj.currentStyle) {
        return obj.currentStyle[attr];
      }
      else {
        return getComputedStyle(obj, false)[attr];
      }
    }

    function setUnitA() {
      document.documentElement.style.fontSize =
        document.documentElement.clientWidth / 10 / fs * 100 + "%";
    }

    var h = null;
    window.addEventListener("resize", function () {
      clearTimeout(h);
      h = setTimeout(setUnitA, 300);
    }, false);
    setUnitA();
  }

  initRem(document, window)

#自适应布局
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0">
  <meta name="apple-mobile-web-app-capable" content="yes">
  <meta name="format-detection" content="telephone=no">
  

#vue、react、angular
* Vue.js
一个用于创建 web 交互界面的库，是一个精简的 MVVM。它通过双向数据绑定把 View 层和 Model 层连接了起来。实际的 DOM 封装和输出格式都被抽象为了Directives 和 Filters

* AngularJS
是一个比较完善的前端MVVM框架，包含模板，数据双向绑定，路由，模块化，服务，依赖注入等所有功能，模板功能强大丰富，自带了丰富的 Angular指令

* react
React 仅仅是 VIEW 层是facebook公司。推出的一个用于构建UI的一个库，能够实现服务器端的渲染。用了virtual dom，所以性能很好。


#说下闭包，闭包用多了会内存泄漏，还有什么也会内存泄漏？
闭包是指有权访问另一个函数作用域中变量的函数
闭包的特性：
  函数内嵌套函数
  内部函数可以引用外层的参数和变量
  参数和变量不会被垃圾回收机制回收

使用闭包主要是为了设计私有的方法和变量。闭包的优点是可以避免全局变量的污染，缺点是闭包会常驻内存，会增大内存使用量，使用不当很容易造成内存泄露。在js中，函数即闭包，只有函数才会产生作用域的概念。

内存泄漏： 1⃣️使用递归，2⃣️setTimeout 的第一个参数使用字符串而非函数的话，会引发内存泄漏


#作用域链
作用域链的作用是保证执行环境里有权访问的变量和函数是有序的，作用域链的变量只能向上访问，变量访问到window对象即被终止，作用域链向下访问变量是不被允许的
简单的说，作用域就是变量与函数的可访问范围，即作用域控制着变量与函数的可见性和生命周期


#JavaScript原型，原型链 ? 有什么特点？
每个对象都会在其内部初始化一个属性，就是prototype(原型).
当我们访问一个对象的属性时，如果这个对象内部不存在这个属性，那么他就会去prototype里找这个属性，这个prototype又会有自己的prototype，于是就这样一直找下去，也就是我们平时所说的（原型链）的概念
instance.constructor.prototype = instance.__proto__

特点：JavaScript对象是通过引用来传递的，我们创建的每个新对象实体中并没有一份属于自己的原型副本。当我们修改原型时，与之相关的对象也会继承这一改变


#javascript如何实现继承
* 构造继承
* 原型继承
* 实例继承
* 拷贝继承

  function Parent(){
    this.name = 'wang';
  }

  function Child(){
    this.age = 28;
  }
  Child.prototype = new Parent();//继承了Parent，通过原型

  var demo = new Child();
  alert(demo.age);
  alert(demo.name); //得到被继承的属性


#谈下对this对象的理解
this总指向函数的直接调用者
如果有new关键字，this指向new出来的那个对象
在事件中，this指向触发这个事件的对象。特殊的是，ie中的attachEvent中的this总指向全局对象window


#事件代理（事件委托）
主要是利用事件冒泡，把原本需要绑定的事件委托给父元素，让父元素担当事件监听的职务，点击子元素时通过e.target 或 e.srcElement判断
优点：1，可以节省大量内存空间  2，可以实现当新增子对象时无需对其绑定


#事件模型
冒泡型事件：当你使用事件冒泡时，子级元素先触发，父级元素后触发
捕获型事件：当你使用事件捕获时，父级元素先触发，子级元素后触发
DOM事件流：同时支持两种事件模型：捕获型事件和冒泡型事件
阻止冒泡：在W3c中，使用stopPropagation()方法；在IE下设置cancelBubble = true
阻止捕获：阻止事件的默认行为，例如click - <a>后的跳转。在W3c中，使用preventDefault（）方法，在IE下设置window.event.returnValue = false


## 请描述一下 cookies，sessionStorage 和 localStorage 的区别？
存储大小：
cookie数据大小不能超过4k
sessionStorage和localStorage虽然也有存储大小的限制，但比cookie大得多，可以达到5M或更大

有期时间：
cookie 设置的cookie过期时间之前一直有效，即使窗口或浏览器关闭
sessionStorage 数据在当前浏览器窗口关闭后自动删除
localStorage 存储持久数据，浏览器关闭后数据不丢失除非主动删除数据

cookie是网站为了标示用户身份而储存在用户本地终端（Client Side）上的数据（通常经过加密）
cookie数据始终在同源的http请求中携带（即使不需要），记会在浏览器和服务器间来回传递
sessionStorage和localStorage不会自动把数据发给服务器，仅在本地保存


## 从浏览器地址栏输入url到显示页面的步骤
浏览器根据请求的URL交给DNS域名解析，找到真实IP，向服务器发起请求；
服务器交给后台处理完成后返回数据，浏览器接收文件（HTML、JS、CSS、图像等）；
浏览器对加载到的资源（HTML、JS、CSS等）进行语法解析，建立相应的内部数据结构（如HTML的DOM）；
载入解析到的资源文件，渲染页面，完成


## HTTP的几种请求方法用途
get: 产生一个TCP数据包, 对于GET方式的请求，浏览器会把http header和data一并发送出去，服务器响应200（返回数据）；
post: 产生两个TCP数据包。而对于POST，浏览器先发送header，服务器响应100 continue，浏览器再发送data，服务器响应200 ok（返回数据）。

GET参数通过URL传递，POST放在Request body中。GET比POST更不安全，因为参数直接暴露在URL上，所以不能用来传递敏感信息。GET请求会被浏览器主动cache，而POST不会，除非手动设置。GET在浏览器回退时是无害的，而POST会再次提交请求。

HTTP的底层是TCP/IP。所以GET和POST的底层也是TCP/IP，也就是说，GET/POST都是TCP链接。GET和POST能做的事情是一样一样的。你要给GET加上request body，给POST带上url参数，技术上是完全行的通的。 

其他还有：put/head/delete/options/trace/connect


## HTTP状态码及其含义
* 1XX：信息状态码
    100 Continue 继续，一般在发送post请求时，已发送了http header之后服务端将返回此信息，表示确认，之后发送具体参数信息
* 2XX：成功状态码
    200 OK 正常返回信息
    201 Created 请求成功并且服务器创建了新的资源
    202 Accepted 服务器已接受请求，但尚未处理
* 3XX：重定向
    301 Moved Permanently 请求的网页已永久移动到新位置。
    302 Found 临时性重定向。
    303 See Other 临时性重定向，且总是使用 GET 请求新的 URI。
    304 Not Modified 自从上次请求后，请求的网页未修改过。
* 4XX：客户端错误
    400 Bad Request 服务器无法理解请求的格式，客户端不应当尝试再次使用相同的内容发起请求。
    401 Unauthorized 请求未授权。
    403 Forbidden 禁止访问。
    404 Not Found 找不到如何与 URI 相匹配的资源。
* 5XX: 服务器错误
    500 Internal Server Error 最常见的服务器端错误。
    503 Service Unavailable 服务器端暂时无法处理请求（可能是过载或维护）


## 如何进行网站性能优化
content方面:
  减少HTTP请求：合并文件、CSS精灵、inline Image
  减少DNS查询：DNS缓存、将资源分布到恰当数量的主机名
  减少DOM元素数量

Server方面:
  使用CDN
  配置ETag
  对组件使用Gzip压缩

css方面:
  将样式表放到页面顶部
  不使用CSS表达式
  使用<link>不使用@import

Javascript方面:
  将脚本放到页面底部
  将javascript和css从外部引入
  压缩javascript和css
  删除不需要的脚本
  减少DOM访问

图片方面:
  优化图片：根据实际颜色需要选择色深、压缩
  优化css精灵
  不要在HTML中拉伸图片


## 前端需要注意哪些seo:
合理的title、description、keywords：搜索对着三项的权重逐个减小，title值强调重点即可，重要关键词出现不要超过2次，而且要靠前，不同页面title要有所不同；
description把页面内容高度概括，长度合适，不可过分堆砌关键词，不同页面description有所不同；keywords列举出重要关键词即可
语义化的HTML代码，符合W3C规范：语义化代码让搜索引擎容易理解网页
重要内容HTML代码放在最前：搜索引擎抓取HTML顺序是从上到下，有的搜索引擎对抓取长度有限制，保证重要内容一定会被抓取
重要内容不要用js输出：爬虫不会执行js获取内容
少用iframe：搜索引擎不会抓取iframe中的内容
非装饰性图片必须加alt
提高网站速度：网站速度是搜索引擎排序的一个重要指标

#如何解决跨域问题?
jsonp、 iframe、window.name、window.postMessage、服务器上设置代理页面

#谈谈你对webpack的看法
WebPack 是一个模块打包工具，你可以使用WebPack管理你的模块依赖，并编绎输出模块们所需的静态文件。它能够很好地管理、打包Web开发中所用到的HTML、Javascript、CSS以及各种静态文件（图片、字体等），让开发过程更加高效。对于不同类型的资源，webpack有对应的模块加载器。webpack模块打包器会分析模块间的依赖关系，最后 生成了优化且合并后的静态资源

#你觉得jQuery源码有哪些写的好的地方
jquery源码封装在一个匿名函数的自执行环境中，有助于防止变量的全局污染，然后通过传入window对象参数，可以使window对象作为局部变量使用，好处是当jquery中访问window对象的时候，就不用将作用域链退回到顶层作用域了，从而可以更快的访问window对象。同样，传入undefined参数，可以缩短查找undefined时的作用域链
jquery将一些原型属性和方法封装在了jquery.prototype中，为了缩短名称，又赋值给了jquery.fn，这是很形象的写法
有一些数组或对象的方法经常能使用到，jQuery将其保存为局部变量以提高访问速度
jquery实现的链式调用可以节约代码，所返回的都是同一个对象，可以提高代码效率

#Ajax原理
Ajax的原理简单来说是在用户和服务器之间加了—个中间层(AJAX引擎)，通过XmlHttpRequest对象来向服务器发异步请求，从服务器获得数据，然后用javascript来操作DOM而更新页面。使用户操作与服务器响应异步化。这其中最关键的一步就是从服务器获得请求数据
Ajax的过程只涉及JavaScript、XMLHttpRequest和DOM。XMLHttpRequest是ajax的核心机制

  * 1. 创建连接
  var xhr = null;
  xhr = new XMLHttpRequest()
  * 2. 连接服务器
  xhr.open('get', url, true)
  * 3. 发送请求
  xhr.send(null);
  * 4. 接受请求
  xhr.onreadystatechange = function(){
    if(xhr.readyState == 4){
      if(xhr.status == 200){
        success(xhr.responseText);
      } else { // fail
        fail && fail(xhr.status);
      }
    }
  }