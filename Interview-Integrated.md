# 尚硅谷前端面试题

## HTML
### 1. H5新特性有哪些？
    * 语义化标签header,nav,footer,aside,article,section
    * 音频、视频API(audio,video)
    * 画布(Canvas) API
    * 地理(Geolocation) API
    * localStorage和sessionStorage
    * webworker, websocket
    * web worker是运行在浏览器后台的js程序，他不影响主程序的运行，是另开的一个js线程，可以用这个线程执行复杂的数据操作，然后把操作结果通过postMessage传递给主线程，这样在进行复杂且耗时的操作时就不会阻塞主线程了。
### 2.两种实现前端路由的方式
#### 第一种：HTML5 History两个新增的API:
 <code>history.pushState</code> 和<code>history.replaceState</code>，可以操作浏览器的历史记录，而不会引起页面的刷新。
#### 第二种：History interface提供的两个新方法
<code>.pushState(stateObject, title, URL)</code>
<code>.replaceState(stateObject, title, URL)</code>
 可以对浏览器历史记录栈进行修改
#### 前端路由的优点:
```
1.比如页面持久性，像大部分音乐网站，你都可以在播放歌曲的同时，跳转到别的页面而音乐没有中断，再比如前后端彻底分离。
2.开发一个前端路由，主要考虑到页面的可插拔、页面的生命周期、内存管理等。
3.从性能和用户体验的层面来比较的话，后端路由每次访问一个新页面的时候都要向服务器发送请求，然后服务器再响应请求，这个过程肯定会有延迟。而前端路由在访问一个新页面的时候仅仅是变换了一下路径而已，没有了网络延迟，对于用户体验来说会有相当大的提升。
```
#### 前端路由的缺点:
```
使用浏览器的前进，后退键的时候会重新发送请求，没有合理地利用缓存。
```
#### 关于Hash：
在前端路由中，Hash就是url中的#,我们需要一个根据监听哈希变化触发的事件(hashchange) 事件。
我们用<code>window.location</code>处理哈希的改变时不会重新渲染页面，而是当作新页面加到历史记录中，这样我们跳转页面就可以在hashchange事件中注册 ajax从而改变页面内容。可以使用<code>window.addEventListener("hashchange", funcRef, false)</code>对hash的改变添加监听事件：
### 3.websocket
WebSocket 使用ws或wss协议，Websocket是一个持久化的协议，相对于HTTP这种非持久的协议来说。WebSocket API最伟大之处在于服务器和客户端可以在给定的时间范围内的任意时刻，相互推送信息。WebSocket并不限于以Ajax(或XHR)方式通信，因为Ajax技术需要客户端发起请求，而WebSocket服务器和客户端可以彼此相互推送信息；XHR受到域的限制，而WebSocket允许跨域通信。
```javascript
// 创建一个Socket实例
var socket = new WebSocket('ws://localhost:8080');
// 打开Socket
socket.onopen = function(event) {
  // 发送一个初始化消息
  socket.send('I am the client and I\'m listening!');
  // 监听消息
  socket.onmessage = function(event) {
    console.log('Client received a message',event);
  };
  // 监听Socket的关闭
  socket.onclose = function(event) {
    console.log('Client notified socket has closed',event);
  };
  // 关闭Socket....
  socket.close()
}
```
### 4.WebSocket如何兼容低浏览器？(阿里)
    1.Adobe Flash Socket
    2.ActiveX HTMLFile (IE)
    3.基于 multipart 编码发送 XHR
    4.基于长轮询的 XHR
### 5.浏览器渲染原理解析
    1.首先渲染引擎下载HTML，解析生成DOM Tree
    2.遇到css标签或JS脚本标签就新起线程去下载他们，并继续构建DOM。(其中css是异步下载同步执行)浏览器引擎通过 DOM Tree 和 CSS Rule Tree 构建 Rendering Tree
    3.通过 CSS Rule Tree 匹配 DOM Tree 进行定位坐标和大小，这个过程称为 Flow 或 Layout 。
    4.最终通过调用Native GUI 的 API 绘制网页画面的过程称为 Paint 。
    5.当用户在浏览网页时进行交互或通过 js 脚本改变页面结构时，以上的部分操作有可能重复运行，此过程称为 Repaint 或 Reflow。 重排是指dom树发生结构变化后，需要重新构建dom结构。 重绘是指dom节点样式改变，重新绘制。 重排一定会带来重绘，重绘不一定有重排。
### 6.如何减少浏览器重排
    将需要多次重排的元素，position属性设为absolute或fixed，这样此元素就脱离了文档流，它的变化不会影响到其他元素。
### 7.iframe有那些缺点？
    1.iframe会阻塞主页面的Onload事件；
    2.搜索引擎的检索程序无法解读这种页面，不利于SEO;
    3.iframe和主页面共享连接池，而浏览器对相同域的连接有限制，所以会影响页面的并行加载。
    4.使用iframe之前需要考虑这两个缺点。如果需要使用iframe，最好是通过javascript
    5.动态给iframe添加src属性值，这样可以绕开以上两个问题。
### 8.Label的作用是什么？是怎么用的？
label标签来定义表单控制间的关系,当用户选择该标签时，浏览器会自动将焦点转到和标签相关的表单控件上。
```html
<label for="Name">Number:</label>
<input type="text" name="Name" id="Name"/>
<label>Date:<input type="text" name="B"/></label>
```
### 9.HTML5的form如何关闭自动完成功能？
    给不想要提示的 form 或某个 input 设置为 autocomplete=off。
### 10.如何实现浏览器内多个标签页之间的通信? (阿里)
    1.WebSocket、SharedWorker；
    2.调用localstorge、cookies等本地存储方式；
     localstorge在另一个浏览上下文里被添加、修改或删除时，它都会触发一个事件，我们通过监听事件，控制它的值来进行页面信息通信；
     注意quirks：Safari 在无痕模式下设置localstorge值时会抛出 QuotaExceededError 的异常；
### 11.页面可见性（Page Visibility API） 可以有哪些用途？
    1.通过 visibilityState 的值检测页面当前是否可见，以及打开网页的时间等;
    2. 在页面被切换到其他后台进程的时候，自动暂停音乐或视频的播放；
### 12.实现不使用 border 画出1px高的线，在不同浏览器的标准模式与怪异模式下都能保持一致的效果。
```html
<div style="height:1px;overflow:hidden;background:red"></div>
```
### 13.title与h1的区别、b与strong的区别、i与em的区别？
* <code>title</code>属性没有明确意义只表示是个标题，<code>h1</code>则表示层次明确的标题，对页面信息的抓取也有很大的影响；
* <code>strong</code>是标明重点内容，有语气加强的含义，使用阅读设备阅读网络时：<code>strong</code>会重读，而<code>b</code>是展示强调内容。
* <code>i</code>内容展示为斜体，<code>em</code>表示强调的文本；
* 自然样式标签: <code>b</code>  <code>i</code>  <code>u</code>  <code>s</code>  <code>pre</code>
* 语义样式标签: <code>strong</code> <code>em</code> <code>ins</code>  <code>del</code>  <code>code</code>
* 应该准确使用语义样式标签, 但不能滥用, 如果不能确定时首选使用自然样式标签。
### 14.你做的页面在哪些流览器测试过？这些浏览器的内核分别是什么?
    IE--------Trident内核
    Firefox---Gecko内核
    Safari----Webkit内核
    Opera-----Blink内核
    QQ,360----Trident Blink双内核
### 15.<code><!Doctype></code>是干什么的？
* <code><!DOCTYPE></code>叫文档声明, <code><!DOCTYPE></code>声明位于文档中的最前面的位置，处于<code>html</code>标签之前。
* 此标签可告知浏览器文档使用哪种 HTML 或 XHTML 规范。（重点：告诉浏览器按照何种规范解析页面）
* IE下如不书写文档声明会使用怪异模式解析网页导致一系列CSS兼容性问题。
### 16.div+css的布局较table布局有什么优点？
    div+css的布局优点：
    1. 结构与样式分离
    2. 代码语义性好
    3. 更符合HTML标准规范
    4. SEO友好
    
    Table布局的适用场景：
    某种原因不方便加载外部CSS的场景，例如邮件正文，此时用table布局可以在无css情况下保持页面布局正常。
### 17.img的alt与title有何异同？strong与em的异同？
    alt(alt text):为不能显示图像、窗体或applets的用户代理（UA），alt属性用来指定替换文字。替换文字的语言由lang属性指定。(在IE浏览器下会在没有title时把alt当成 tool tip显示)，title(tool tip):该属性为设置该属性的元素提供建议性的信息。
    
    em:表现为斜体，语义表；示强调strong:表现为粗体，语义为强烈语气，强调程度超过em
### 18.简述一下src与href的区别
    <script src =”js.js”></script>
    src(是source的缩写)用于替换当前元素，href用于在当前文档和引用资源之间确立联系，指向外部资源的位置，指向的内容将会嵌入到文档中当前标签所在位置；在请求src资源时会将其指向的资源下载并应用到文档内，例如js脚本，img图片和frame等元素。
    当浏览器解析到该元素时，会暂停其他资源的下载和处理，直到将该资源加载、编译、执行完毕，图片和框架等元素也如此，类似于将所指向资源嵌入当前标签内。这也是为什么将js脚本放在底部而不是头部。
    
    <link href=”common.css” rel=”stylesheet”/>
    href是Hypertext Reference的缩写，指向网络资源所在位置，建立和当前元素（锚点）或当前文档（链接）之间的链接，如果我们在文档中添加
    那么浏览器会识别该文档为css文件，就会并行下载资源并且不会停止对当前文档的处理。这也是为什么建议使用link方式来加载css，而不是使用@import方式。

### 19.知道的网页制作会用到的图片格式有哪些？
    png-8，png-24，jpeg，gif，svg，Webp（谷歌开发的一种旨在加快图片加载速度的图片格式。图片压缩体积大约只有JPEG的2/3，并能节省大量的服务器带宽资源和数据空间。Facebook Ebay等知名网站已经开始测试并使用WebP格式。
### 20.在css/js代码上线之后开发人员经常会优化性能，从用户刷新网页开始，一次js请求一般情况下有哪些地方会有缓存处理？
    dns缓存，cdn缓存，浏览器缓存，服务器缓存。
### 21.一个页面上有大量的图片（大型电商网站），加载很慢，你有哪些方法优化这些图片的加载，给用户更好的体验。
    * 图片懒加载，在页面上的未可视区域可以添加一个滚动条事件，判断图片位置与浏览器顶端的距离与页面的距离，如果前者小于后者，优先加载。
    * 如果为幻灯片、相册等，可以使用图片预加载技术，将当前展示图片的前一张和后一张优先下载。
    * 如果图片为css图片，可以使用CSSsprite，SVGsprite，Iconfont、Base64等技术。
    * 如果图片过大，可以使用特殊编码的图片，加载时会先加载一张压缩的特别厉害的缩略图，以提高用户体验。
    * 如果图片展示区域小于图片的真实大小，则因在服务器端根据业务需要先行进行图片压缩，图片压缩后大小与展示一致。
### 22.你如何理解HTML结构的语义化？
    HTML结构语义化：
      * 更符合W3C统一的规范标准，是技术趋势。
      * 没有样式时浏览器的默认样式也能让页面结构很清晰。
      * 对功能障碍用户友好。屏幕阅读器（如果访客有视障）会完全根据你的标记来“读”你的网页。
      * 对其他非主流终端设备友好。例如机顶盒、PDA、各种移动终端。
      * 对SEO友好。
### 23.谈谈以前端角度出发做好SEO需要考虑什么？
    搜索引擎主要以:
    外链数量和质量
    网页内容和结构
    来决定某关键字下的网页搜索排名。
    
    前端应该注意网页结构和内容方面的情况：
    Meta标签优化
    主要包括主题（Title)，网站描述(Description)。还有一些其它的隐藏文字比如Author（作者），Category（目录），Language（编码语种）等。
    符合W3C规范的语义性标签的使用。
    
    如何选取关键词并在网页中放置关键词
    搜索就得用关键词。关键词分析和选择是SEO最重要的工作之一。首先要给网站确定主关键词（一般在5个上下），然后针对这些关键词进行优化，包括关键词密度（Density），相关度（Relavancy），突出性（Prominency）等等。
### 24.html5有哪些新特性、移除了那些元素？如何处理HTML5新标签的浏览器兼容问题？如何区分 HTML 和 HTML5？
    新特性：
      1. 拖拽释放(Drag and drop) API 
      2. 语义化更好的内容标签（header,nav,footer,aside,article,section）
      3. 音频、视频API(audio,video)
      4. 画布(Canvas) API
      5. 地理(Geolocation) API
      6. 本地离线存储 localStorage 长期存储数据，浏览器关闭后数据不丢失；
      7. sessionStorage 的数据在浏览器关闭后自动删除
      8. 表单控件，calendar、date、time、email、url、search  
      9. 新的技术webworker, websocket, Geolocation
    移除的元素：
      1. 纯表现的元素：basefont，big，center，font, s，strike，tt，u；
      2. 对可用性产生负面影响的元素：frame，frameset，noframes；
    支持HTML5新标签：
      1. IE8/IE7/IE6支持通过 document.createElement 方法产生的标签，可以利用这一特性让这些浏览器支持 HTML5 新标签，浏览器支持新标签后，还需要添加标签默认的样式（当然最好的方式是直接使用成熟的框架、使用最多的是html5shim框架）：
      <!--[if lt IE 9]>
      <script> src="http://html5shim.googlecode.com/svn/trunk/html5.js"</script>
      <![endif]-->
    如何区分： 
      DOCTYPE声明新增的结构元素、功能元素
### 25.HTML5 Canvas 元素有什么用？
Canvas 元素用于在网页上绘制图形，该元素标签强大之处在于可以直接在 HTML 上进行图形操作。
### 26.如何在 HTML5 页面中嵌入音频?
```html
HTML 5 包含嵌入音频文件的标准方式，支持的格式包括 MP3、Wav 和 Ogg：
<audio controls> 
  <source src="jamshed.mp3" type="audio/mpeg"> 
   Your browser doesn't support audio embedding feature. 
</audio>
```

### 27.如何在 HTML5 页面中嵌入视频？
```html
和音频一样，HTML5 定义了嵌入视频的标准方法，支持的格式包括：MP4、WebM 和 Ogg：
<video width="450" height="340" controls> 
  <source src="jamshed.mp4" type="video/mp4"> 
   Your browser does'nt support video embedding feature. 
</video>
```

### 28.HTML5引入什么新的表单属性？
    Datalist,datetime,output,keygen,date,month,week,time,number,range,emailurl
## CSS
### 1.box-sizing
    box-sizing属性主要用来控制元素的盒模型的解析模式。默认值是content-box。
    content-box：让元素维持W3C的标准盒模型。元素的宽度/高度由border + padding + content的宽度/高度决定，设置width/height属性指的是content部分的宽/高
    border-box：让元素维持IE传统盒模型（IE6以下版本和IE6~7的怪异模式）。设置width/height属性指的是border + padding + content
    应用场景：统一风格的表单元素 表单中有一些input元素其实还是展现的是传统IE盒模型，带有一些默认的样式，而且在不同平台或者浏览器下的表现不一，造成了表单展现的差异。此时我们可以通过box-sizing属性来构建一个风格统一的表单元素。
### 2.水平垂直居中的方法
```css
行内布局:line-height + text-align 或者 vertical-align + text-align
块布局: position absolute + margin auto position absolute + negative margin position absolute + translate(-50%, -50%)

/*父容器子容器不确定宽高的块级元素，做上下居中*/
/*1.flex*/
#wrap{display:flex;justify-content:center;align-items:center;}
/*2.tabel*/
.parent {display: table-cell;text-align: center;vertical-align: middle;}
.child {display: inline-block;} //防止块级元素宽度独占一行，内联元素可不设置
/*3.absolute+transform*/
.parent {position: relative;}
.child {position: absolute;left: 50%;top: 50%;transform: translate(-50%, -50%);}
/*4.webkit-box*/
.parent{position: relative;display: -webkit-box;-webkit-box-align: center;-webkit-box-pack: center;}
```
### 3.实现左边定宽右边自适应效果
    1.table(父级元素)与tabel-cell（两个子集元素）
    2.flex(父级元素)+flex :1（右边子元素）
    3.左边定宽，并且左浮动；右边设置距离左边的宽度
    4.左边定宽，左边设置position:absolute；右边设置距离左边的宽度
### 4.三列布局（中间固定两边自适应宽度）
    1. 采用浮动布局（左边左浮动，右边右浮动，中间margin：0 宽度值）
    2. 绝对定位方式（左右绝对定位，左边left0右边right0，中间上同）
### 5.BFC（Block Formatting Contexts）块级格式化上下文
    块格式化上下文（block formatting context） 是页面上的一个独立的渲染区域，容器里面的子元素不会在布局上影响到外面的元素。它是决定块盒子的布局及浮动元素相互影响的一个因素。
    
    下列情况将创建一个块格式化上下文：
    ① float
    ② overflow
    ③ display（display为inline-block、table-cell）
    ④ position（absolute 或 fixed）
    BFC的作用
      1.清除内部浮动：对子元素设置浮动后，父元素会发生高度塌陷，也就是父元素的高度变为0。解决这个问题，只需要把把父元素变成一个BFC就行了。常用的办法是给父元素设置overflow:hidden。
      2.上下margin重合问题，可以通过触发BFC来解决
### 6.清除浮动元素的方法和各自的优缺点
    清除浮动，实际上是清除父元素的高度塌陷。因为子元素脱离了父元素的文档流，所以，父元素失去了高度，导致了塌陷。要解决这个问题，就是让父元素具有高度。
    
    浮动元素的特性： 在正常布局中位于该浮动元素之下的内容，此时会围绕着浮动元素，填满其右侧的空间。浮动到右侧的元素，其他内容将从左侧环绕它（浮动元素影响的不仅是自己，它会影响周围的元素对其进行环绕。float仍会占据其位置，position:absolute不占用页面空间 会有重叠问题 ）
    
    1.在浮动元素末尾添加空标签清除浮动 clear:both （缺点：增加无意义标签）
    <div style="clear:both;"></div>
    2.给父元素设置 overflow:auto属性 
    3.after伪元素

### 7.css实现自适应正方形
    方案一：CSS3 vw 单位
    方案二：设置垂直方向的padding撑开容器
    方案三：利用伪元素的 margin(padding)-top 撑开容器
### 8.position的值
    1. absolute :生成绝对定位的元素， 相对于最近一级的 定位不是 static 的父元素来进行定位。
    2. fixed （老IE不支持）生成绝对定位的元素，通常相对于浏览器窗口或 frame 进行定位。
    3. relative 生成相对定位的元素，相对于其在普通流中的位置进行定位。
    4. static 默认值。没有定位，元素出现在正常的流中
    5. sticky 生成粘性定位的元素，容器的位置根据正常文档流计算得出
### 9.如何在页面上实现一个圆形的可点击区域？
    1、map+area或者svg
    2、border-radius
    3、纯js实现 需要求一个点在不在圆上简单算法、获取鼠标坐标等等
### 10.介绍一下标准的CSS的盒子模型？低版本IE的盒子模型有什么不同的？
    （1）有两种， IE 盒子模型、W3C 盒子模型；
    （2）盒模型： 内容(content)、填充(padding)、边界(margin)、 边框(border)；
    （3）区  别： IE的content部分把 border 和 padding计算了进去;
### 11.CSS选择符有哪些？哪些属性可以继承？
#### 1.基本选择器
     *   通用元素选择器，匹配任何元素
     E   标签选择器，匹配所有使用E标签的元素
     .info  class选择器，匹配所有class属性中包含info的元素
     #footer  id选择器，匹配所有id属性等于footer的元素
#### 2.多元素的组合选择器
     E,F     多元素选择器，同时匹配所有E元素或F元素，E和F之间用逗号分隔
     E F    后代元素选择器，匹配所有属于E元素后代的F元素，E和F之间用空格分隔
     E > F 子元素选择器，匹配所有E元素的子元素F
     E + F 毗邻元素选择器，匹配所有紧随E元素之后的同级元素F
#### 3.CSS 2.1 属性选择器
     E[att]     匹配所有具有att属性的E元素，不考虑它的值。（注意：E在此处可以省略，比如"[cheacked]"。以下同。）
     E[att=val]  匹配所有att属性等于"val"的E元素
     E[att~=val]    匹配所有att属性具有多个空格分隔的值、其中一个值等于"val"的E元素
     E[att|=val] 匹配所有att属性具有多个连字号分隔（hyphen-separated）的值、其中一个值以"val"开头的E元素，主要用于lang属性，比如"en"、"en-us"、"en-gb"等等
#### 4.CSS 2.1中的伪类
     E:first-child    匹配父元素的第一个子元素
     E:link匹配所有未被点击的链接
     E:visited     匹配所有已被点击的链接
     E:active  匹配鼠标已经其上按下、还没有释放的E元素
     E:hover  匹配鼠标悬停其上的E元素
     E:focus  匹配获得当前焦点的E元素
     E:lang(c)    匹配lang属性等于c的E元素
#### 5.CSS 2.1中的伪元素
     E:first-line  匹配E元素的第一行
     E:first-letter   匹配E元素的第一个字母
     E:before 在E元素之前插入生成的内容
     E:after   在E元素之后插入生成的内容
#### 6.CSS 3的同级元素通用选择器
     E ~ F 匹配任何在E元素之后的同级F元素
#### 7.CSS 3 属性选择器
     E[att^="val"] 属性att的值以"val"开头的元素
     E[att$="val"]  属性att的值以"val"结尾的元素
     E[att*="val"]  属性att的值包含"val"字符串的元素
#### 8.CSS 3中与用户界面有关的伪类
     E:enabled   匹配表单中激活的元素
     E:disabled  匹配表单中禁用的元素
     E:checked   匹配表单中被选中的radio（单选框）或checkbox（复选框）元素
     E::selection     匹配用户当前选中的元素
#### 9.CSS 3中的结构性伪类
     E:root    匹配文档的根元素，对于HTML文档，就是HTML元素
     E:nth-child(n) 匹配其父元素的第n个子元素，第一个编号为1
     E:nth-last-child(n)   匹配其父元素的倒数第n个子元素，第一个编号为1
     E:nth-of-type(n) 与:nth-child()作用类似，但是仅匹配使用同种标签的元素
     E:nth-last-of-type(n)   与:nth-last-child() 作用类似，但是仅匹配使用同种标签的元素
     E:last-child 匹配父元素的最后一个子元素，等同于:nth-last-child(1)
     E:first-of-type匹配父元素下使用同种标签的第一个子元素，等同于:nth-of-type(1)
     E:last-of-type 匹配父元素下使用同种标签的最后一个子元素，等同于:nth-last-of-type(1)
     E:only-child    匹配父元素下仅有的一个子元素，等同于:first-child:last-child或 :nth-child(1):nth-last-child(1)
     E:only-of-type    匹配父元素下使用同种标签的唯一一个子元素，等同于:first-of-type:last-of-type或 :nth-of-type(1):nth-last-of-type(1)
    E:empty 匹配一个不包含任何子元素的元素，注意，文本节点也被看作子元素
#### 10.CSS 3的反选伪类
    E:not(s) 匹配不符合当前选择器的任何元素
#### 11.CSS 3中的 :target 伪类
    E:target 匹配文档中特定"id"点击后的效果
      *可继承的样式： font-size font-family color, UL LI DL DD DT;
      *不可继承的样式：border padding margin width height ;
### 12.CSS优先级算法如何计算？
    *优先级就近原则，同权重情况下样式定义最近者为准;
    *载入样式以最后载入的定位为准;
    
    优先级为:
      同权重: 内联样式表（标签内部）> 嵌入样式表（当前文件中）> 外部样式表（外部文件中）。
      !important >  id > class > tag
      important 比 内联优先级高
### 13.CSS3新增伪类有那些？
    举例：
    p:first-of-type     选择属于其父元素的首个 <p> 元素的每个 <p> 元素。
    p:last-of-type 选择属于其父元素的最后 <p> 元素的每个 <p> 元素。
      p:only-of-type  选择属于其父元素唯一的 <p> 元素的每个 <p> 元素。
    p:only-child        选择属于其父元素的唯一子元素的每个 <p> 元素。
    p:nth-child(2) 选择属于其父元素的第二个子元素的每个 <p> 元素。
    
    ::after             在元素之前添加内容,也可以用来做清除浮动。
    ::before          在元素之后添加内容
      :enabled       
    :disabled        控制表单控件的禁用状态。
    :checked        单选框或复选框被选中。
### 14.display有哪些值？说明他们的作用。
    block          块类型。默认宽度为父元素宽度，可设置宽高，换行显示。
    none          缺省值。象行内元素类型一样显示。
    inline         行内元素类型。默认宽度为内容宽度，不可设置宽高，同行显示。
    inline-block  默认宽度为内容宽度，可以设置宽高，同行显示。
    list-item     象块类型元素一样显示，并添加样式列表标记。
    table          此元素会作为块级表格来显示。
    inherit        规定应该从父元素继承 display 属性的值。
### 15.CSS3有哪些新特性？
    新增各种CSS选择器    （: not(.input)：所有 class 不是“input”的节点）
    圆角           （border-radius:8px）
    多列布局     （multi-column layout）
    阴影和反射   （Shadow\Reflect）
    文字特效      （text-shadow、）
    文字渲染      （Text-decoration）
    线性渐变      （gradient）
    旋转            （transform）
    缩放,定位,倾斜,动画,多背景
    例如:transform:\scale(0.85,0.90)\ translate(0px,-30px)\ skew(-9deg,0deg)\Animation:
### 16.请解释一下CSS3的Flexbox（弹性盒布局模型）,以及适用场景？
     一个用于页面布局的全新CSS3功能，Flexbox可以把列表放在同一个方向（从上到下排列，从左到右），并让列表能延伸到占用可用的空间。
     较为复杂的布局还可以通过嵌套一个伸缩容器（flex container）来实现。
     采用Flex布局的元素，称为Flex容器（flex container），简称"容器"。
     它的所有子元素自动成为容器成员，称为Flex项目（flex item），简称"项目"。
     常规布局是基于块和内联流方向，而Flex布局是基于flex-flow流可以很方便的用来做局中，能对不同屏幕大小自适应。
     在布局上有了比以前更加灵活的空间。
      
     具体：http://www.w3cplus.com/css3/flexbox-basics.html
### 17.用纯CSS创建一个三角形的原理是什么？
    把上、左、右三条边隐藏掉（颜色设为 transparent）
    #demo {
      width: 0;
      height: 0;
      border-width: 20px;
      border-style: solid;
      border-color: transparent transparent red transparent;
    }
### 18.一个满屏 品 字布局 如何设计?
    简单的方式：
      上面的div宽100%，
      下面的两个div分别宽50%，
      然后用float或者inline使其不换行即可
### 19.css多列等高如何实现？
    利用padding-bottom|margin-bottom正负值相抵；
    设置父容器设置超出隐藏（overflow:hidden），这样子父容器的高度就还是它里面的列没有设定padding-bottom时的高度，
    当它里面的任 一列高度增加了，则父容器的高度被撑到里面最高那列的高度，
    其他比这列矮的列会用它们的padding-bottom补偿这部分高度差。
### 20.经常遇到的浏览器的兼容性有哪些？原因，解决方法是什么，常用hack的技巧 ？
    * png24位的图片在iE6浏览器上出现背景，解决方案是做成PNG8.
    * 浏览器默认的margin和padding不同。解决方案是加一个全局的*{margin:0;padding:0;}来统一。
    * IE6双边距bug:块属性标签float后，又有横行的margin情况下，在ie6显示margin比设置的大。
      浮动ie产生的双倍距离 #box{ float:left; width:10px; margin:0 0 0 100px;}
      这种情况之下IE会产生20px的距离，解决方案是在float的标签样式控制中加入 ——_display:inline;将其转化为行内属性。(_这个符号只有ie6会识别)
      渐进识别的方式，从总体中逐渐排除局部。
      首先，巧妙的使用“\9”这一标记，将IE游览器从所有情况中分离出来。
      接着，再次使用“+”将IE8和IE7、IE6分离开来，这样IE8已经独立识别。 
    
    css
        .bb{
            background-color:red;/*所有识别*/
          background-color:#00deff\9; /*IE6、7、8识别*/
          +background-color:#a200ff;/*IE6、7识别*/
          _background-color:#1e0bd1;/*IE6识别*/
        }
    
    * IE下,可以使用获取常规属性的方法来获取自定义属性,
      也可以使用getAttribute()获取自定义属性;
      Firefox下,只能使用getAttribute()获取自定义属性。
      解决方法:统一通过getAttribute()获取自定义属性。
      
    * IE下,even对象有x,y属性,但是没有pageX,pageY属性;
      Firefox下,event对象有pageX,pageY属性,但是没有x,y属性。
      
    * 解决方法：（条件注释）缺点是在IE浏览器下可能会增加额外的HTTP请求数。
      
    * Chrome 中文界面下默认会将小于 12px 的文本强制按照 12px 显示,
     可通过加入 CSS 属性 -webkit-text-size-adjust: none; 解决。
    
    超链接访问过后hover样式就不出现了 被点击访问过的超链接样式不在具有hover和active了解决方法是改变CSS属性的排列顺序:
    L-V-H-A :  
      a:link {} 
      a:visited {} 
      a:hover {} 
      a:active {}
### 21.li与li之间有看不见的空白间隔是什么原因引起的？有什么解决办法？
    行框的排列会受到中间空白（回车\空格）等的影响，因为空格也属于字符,这些空白也会被应用样式，占据空间，所以会有间隔，把字符大小设为0，就没有空格了。
### 22.为什么要初始化CSS样式。
```css
- 因为浏览器的兼容问题，不同浏览器对有些标签的默认值是不同的，如果没对CSS初始化往往会出现浏览器之间的页面显示差异。
- 当然，初始化样式会对SEO有一定的影响，但鱼和熊掌不可兼得，但力求影响最小的情况下初始化。
最简单的初始化方法： * {padding: 0; margin: 0;} （强烈不建议）
淘宝的样式初始化代码：
body, h1, h2, h3, h4, h5, h6, hr, p, blockquote, dl, dt, dd, ul, ol, li, pre, form, fieldset, legend, button, input, textarea, th, td { margin:0; padding:0; }
body, button, input, select, textarea { font:12px/1.5tahoma, arial, \5b8b\4f53; }
h1, h2, h3, h4, h5, h6{ font-size:100%; }
address, cite, dfn, em, var { font-style:normal; }
code, kbd, pre, samp { font-family:couriernew, courier, monospace; }
small{ font-size:12px; }
ul, ol { list-style:none; }
a { text-decoration:none; }
a:hover { text-decoration:underline; }
sup { vertical-align:text-top; }
sub{ vertical-align:text-bottom; }
legend { color:#000; }
fieldset, img { border:0; }
button, input, select, textarea { font-size:100%; }
table { border-collapse:collapse; border-spacing:0; }
```
### 23.absolute的containing block(容器块)计算方式跟正常流有什么不同？
    无论属于哪种，都要先找到其祖先元素中最近的 position 值不为 static 的元素，然后再判断：
    1、若此元素为 inline 元素，则 containing block 为能够包含这个元素生成的第一个和最后一个 inline box 的 padding box (除 margin, border 外的区域) 的最小矩形；
    2、否则,则由这个祖先元素的 padding box 构成。
    如果都找不到，则为 initial containing block。
      
    补充：
    1. static(默认的)/relative：简单说就是它的父元素的内容框（即去掉padding的部分）
    2. absolute: 向上找最近的定位为absolute/relative的元素
    3. fixed: 它的containing block一律为根元素(html/body)，根元素也是initial containing block
### 24.CSS里的visibility属性有个collapse属性值是干嘛用的？在不同浏览器下以后什么区别？
    对于普通元素visibility:collapse会将元素完全隐藏,不占据页面布局空间,与display:none表现相同
    如果目标元素为table,visibility:collapse;将table隐藏,但是会占据页面布局空间. 仅在Firefox下起作用,IE会显示元素,Chrome会将元素隐藏,但是占据空间.
### 25.position跟display、margin collapse、overflow、float这些特性相互叠加后会怎么样？
    如果元素的display为none,那么元素不被渲染,position,float不起作用,如果元素拥有position:absolute;或者position:fixed;属性那么元素将为绝对定位,float不起作用.如果元素float属性不是none,元素会脱离文档流,根据float属性值来显示.有浮动,绝对定位,inline-block属性的元素,margin不会和垂直方向上的其他元素margin折叠.
### 26.对BFC规范(块级格式化上下文：block formatting context)的理解？
    （W3C CSS 2.1 规范中的一个概念,它是一个独立容器，决定了元素如何对其内容进行定位,以及与其他元素的关系和相互作用。）
    一个页面是由很多个 Box 组成的,元素的类型和 display 属性,决定了这个 Box 的类型。
    不同类型的 Box,会参与不同的 Formatting Context（决定如何渲染文档的容器）,因此Box内的元素会以不同的方式渲染,也就是说BFC内部的元素和外部的元素不会互相影响。
### 27. 请解释一下为什么需要清除浮动？清除浮动的方式
```css
清除浮动是为了清除使用浮动元素产生的影响。浮动的元素，高度会塌陷，而高度的塌陷使我们页面后面的布局不能正常显示。 
 1、父级div定义height；
2、父级div 也一起浮动；
3、常规的使用一个class；
  .clearfix::before, .clearfix::after {
      content: " ";
      display: table;
  }
  .clearfix::after {
      clear: both;
  }
  .clearfix {
      *zoom: 1;
  }

4、SASS编译的时候，浮动元素的父级div定义伪类:after
    &::after,&::before{
        content: " ";
          visibility: hidden;
          display: block;
          height: 0;
          clear: both;
    }

  解析原理：
  1) display:block 使生成的元素以块级元素显示,占满剩余空间;
  2) height:0 避免生成内容破坏原有布局的高度。
  3) visibility:hidden 使生成的内容不可见，并允许可能被生成内容盖住的内容可以进行点击和交互;
  4）通过 content:"."生成内容作为最后一个元素，至于content里面是点还是其他都是可以的，例如oocss里面就有经典的 content:".",有些版本可能content 里面内容为空,一丝冰凉是不推荐这样做的,firefox直到7.0 content:”" 仍然会产生额外的空隙；
  5）zoom：1 触发IE hasLayout。

通过分析发现，除了clear：both用来闭合浮动的，其他代码无非都是为了隐藏掉content生成的内容，这也就是其他版本的闭合浮动为什么会有font-size：0，line-height：0。
```
### 28.什么是外边距合并？
    外边距合并指的是，当两个垂直外边距相遇时，它们将形成一个外边距。
    合并后的外边距的高度等于两个发生合并的外边距的高度中的较大者。
    w3school介绍网址： http://www.w3school.com.cn/css/css_margin_collapsing.asp
### 29.zoom:1的清除浮动原理?
    清除浮动，触发hasLayout；
    Zoom属性是IE浏览器的专有属性，它可以设置或检索对象的缩放比例。解决ie下比较奇葩的bug。
    譬如外边距（margin）的重叠，浮动清除，触发ie的haslayout属性等。
      
    来龙去脉大概如下：
    当设置了zoom的值之后，所设置的元素就会就会扩大或者缩小，高度宽度就会重新计算了，这里一旦改变zoom值时其实也会发生重新渲染，运用这个原理，也就解决了ie下子元素浮动时候父元素不随着自动扩大的问题。
      
    Zoom属是IE浏览器的专有属性，火狐和老版本的webkit核心的浏览器都不支持这个属性。然而，zoom现在已经被逐步标准化，出现在 CSS 3.0 规范草案中。
      
    目前非ie由于不支持这个属性，它们又是通过什么属性来实现元素的缩放呢？
    可以通过css3里面的动画属性scale进行缩放。
### 30.CSS优化、提高性能的方法有哪些？
    关键选择器（key selector）。选择器的最后面的部分为关键选择器（即用来匹配目标元素的部分）；
    如果规则拥有 ID 选择器作为其关键选择器，则不要为规则增加标签。过滤掉无关的规则（这样样式系统就不会浪费时间去匹配它们了）；
    提取项目的通用公有样式，增强可复用性，按模块编写组件；增强项目的协同开发性、可维护性和可扩展性;
    使用预处理工具或构建工具（gulp对css进行语法检查、自动补前缀、打包压缩、自动优雅降级）；
### 31.怎么让Chrome支持小于12px 的文字？
    1、用图片：如果是内容固定不变情况下，使用将小于12px文字内容切出做图片，这样不影响兼容也不影响美观。
    2、使用12px及12px以上字体大小：为了兼容各大主流浏览器，建议设计美工图时候设置大于或等于12px的字体大小，如果是接单的这个时候就需要给客户讲解小于12px浏览器不兼容等事宜。
    3、继续使用小于12px字体大小样式设置：如果不考虑chrome可以不用考虑兼容，同时在设置小于12px对象设置-webkit-text-size-adjust:none，做到最大兼容考虑。
    4、使用12px以上字体：为了兼容、为了代码更简单 从新考虑权重下兼容性。
### 32.position:fixed;在android下无效怎么处理？
    fixed的元素是相对整个页面固定位置的，你在屏幕上滑动只是在移动这个所谓的viewport，
    原来的网页还好好的在那，fixed的内容也没有变过位置，
    所以说并不是iOS不支持fixed，只是fixed的元素不是相对手机屏幕固定的。
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0, user-scalable=no"/>

### 33.如果需要手动写动画，你认为最小时间间隔是多久，为什么？（阿里）
    多数显示器默认频率是60Hz，即1秒刷新60次，所以理论上最小间隔为1/60＊1000ms ＝ 16.7ms
### 34.display:inline-block 什么时候会显示间隙？(携程)
    移除空格、使用margin负值、使用font-size:0、letter-spacing、word-spacing
### 35.有哪项方式可以对一个DOM设置它的CSS样式？
    外部样式表，引入一个外部css文件
    内部样式表，将css代码放在 <head> 标签内部
    内联样式，将css样式直接定义在 HTML 元素内部
### 36.超链接访问过后hover样式就不出现的问题是什么？如何解决？
    被点击访问过的超链接样式不再具有hover和active了,解决方法是改变CSS属性的排列顺序: L-V-H-A（link,visited,hover,active）
### 37.什么是Css Hack？ie6,7,8的hack分别是什么？
```css
针对不同的浏览器写不同的CSS code的过程，就是CSS hack。
示例如下：
   #test{   
        background-color:yellow;    /*ie8*/
        +background-color:pink;        /*ie7*/
        _background-color:orange;       /*ie6*/    }  

更好的方式是使用IE条件判断语句：
<!–[if lte IE 6]>
内容
<![endif]–> 
等
```
### 38.什么是外边距重叠？重叠的结果是什么？
    外边距重叠就是margin-collapse。
    在CSS当中，相邻的两个盒子（可能是兄弟关系也可能是祖先关系）的外边距可以结合成一个单独的外边距。这种合并外边距的方式被称为折叠，并且因而所结合成的外边距称为折叠外边距。
    折叠结果遵循下列计算规则：
    两个相邻的外边距都是正数时，折叠结果是它们两者之间较大的值。
    两个相邻的外边距都是负数时，折叠结果是两者绝对值的较大值。
    两个外边距一正一负时，折叠结果是两者的相加的和。
### 39.rgba()和opacity的透明效果有什么不同？
    rgba()和opacity都能实现透明效果，但最大的不同是opacity作用于元素，以及元素内的所有内容的透明度，
    而rgba()只作用于元素的颜色或其背景色。（设置rgba透明的元素的子元素不会继承透明效果！）
### 40.如何垂直居中一个浮动元素？
```css
// 方法一：已知元素的高宽
#div1{
    background-color:#6699FF;
    width:200px;
    height:200px;
    position: absolute;        /*父元素需要相对定位*/
    top: 50%;
    left: 50%;
    margin-top:-100px ;   /*二分之一的height，width*/
    margin-left: -100px;
    }
//方法二:未知元素的高宽
  #div1{
    width: 200px;
    height: 200px;
    background-color: #6699FF;

    margin:auto;
    position: absolute;        /*父元素需要相对定位*/
    left: 0;
    top: 0;
    right: 0;
    bottom: 0;
    }
/*如何垂直居中一个<img>?（用更简便的方法。）*/

#container     /*<img>的容器设置如下*/
  {
    display:table-cell;
    text-align:center;
    vertical-align:middle;
  }
```
### 41.px和em的区别
    px和em都是长度单位，区别是：
    px值固定，容易计算。
    em值不固定，是相对单位，其相对应父级元素的字体大小会调整
### 42.描述一个”reset”的CSS文件并如何使用它。知道normalize.css吗？你了解他们的不同之处？
    Reset样式的目的是清除浏览器某些默认样式，方便css书写，例如：
    *｛margin:0;padding:0;list-style:none;｝
    Normalize的理念与reset不同，它并不清除浏览器默认样式，而是尽量将所有浏览器的默认样式统一。
### 43.Sass、LESS是什么？大家为什么要使用他们？
    他们是CSS预处理器。他是CSS上的一种抽象层。他们是一种特殊的语法/语言编译成CSS。
    例如Less是一种动态样式语言. 将CSS赋予了动态语言的特性，如变量，继承，运算， 函数. LESS 既可以在客户端上运行 (支持IE 6+, Webkit, Firefox)，也可一在服务端运行 (借助 Node.js)。
    为什么要使用它们？
    结构清晰，便于扩展。
    可以方便地屏蔽浏览器私有语法差异。这个不用多说，封装对浏览器语法差异的重复处理，减少无意义的机械劳动。
    可以轻松实现多重继承。
    完全兼容 CSS 代码，可以方便地应用到老项目中。LESS 只是在 CSS 语法上做了扩展，所以老的 CSS 代码也可以与 LESS 代码一同编译。
### 44.为什么要初始化样式？
    用于浏览器默认css样式的存在并且不同浏览器对相同HTML标签的默认样式不同，若不初始化会造成不同浏览器之间的显示差异。
### 45.IE的双边距BUG：块级元素float后设置横向margin，ie6显示的margin比设置的较大。
    解决：加入_display：inline
### 46.html常见兼容性问题？
    1.双边距BUG float引起的  使用display
    2.3像素问题 使用float引起的 使用dislpay:inline -3px  
    3.超链接hover 点击后失效  使用正确的书写顺序 link visited hover active
    4.IE z-index问题 给父级添加position:relative
    5.Png 透明 使用js代码 改
    6.Min-height 最小高度 ！Important 解决’
    7.select 在ie6下遮盖 使用iframe嵌套
    8.为什么没有办法定义1px左右的宽度容器（IE6默认的行高造成的，使用over:hidden,zoom:0.08 line-height:1px）
    9.IE5-8不支持opacity，解决办法：
    .opacity {
        opacity: 0.4
        filter: alpha(opacity=60); /* for IE5-7 */
        -ms-filter: "progid:DXImageTransform.Microsoft.Alpha(Opacity=60)"; /* for IE 8*/
    }
    10. IE6不支持PNG透明背景，解决办法: IE6下使用gif图片
### 47.对WEB标准以及W3C的理解与认识
    标签闭合、标签小写、不乱嵌套、提高搜索机器人搜索几率、使用外 链css和js脚本、结构行为表现的分离、文件下载与页面速度更快、内容能被更多的用户所访问、内容能被更广泛的设备所访问、更少的代码和组件，容易维 护、改版方便，不需要变动页面内容、提供打印版本而不需要复制内容、提高网站易用性。
### 48.行内元素和块级元素的具体区别是什么？行内元素的padding和margin可设置吗？
    块级元素(block)特性：
      总是独占一行，表现为另起一行开始，而且其后的元素也必须另起一行显示;
      宽度(width)、高度(height)、内边距(padding)和外边距(margin)都可控制;
    内联元素(inline)特性：
      和相邻的内联元素在同一行;
      宽度(width)、高度(height)、内边距的top/bottom(padding-top/padding-bottom)和外边距的top/bottom(margin-top/margin-bottom)都不可改变（也就是padding和margin的left和right是可以设置的），就是里面文字或图片的大小。
    
    那么问题来了，浏览器还有默认的天生inline-block元素（拥有内在尺寸，可设置高宽，但不会自动换行），有哪些？
    答案：<input> 、<img> 、<button> 、<textarea> 、<label>
### 49.什么是外边距重叠？重叠的结果是什么？
    答案：
    外边距重叠就是margin-collapse。
    在CSS当中，相邻的两个盒子（可能是兄弟关系也可能是祖先关系）的外边距可以结合成一个单独的外边距。这种合并外边距的方式被称为折叠，并且因而所结合成的外边距称为折叠外边距。
    折叠结果遵循下列计算规则：
    两个相邻的外边距都是正数时，折叠结果是它们两者之间较大的值。
    两个相邻的外边距都是负数时，折叠结果是两者绝对值的较大值。
    两个外边距一正一负时，折叠结果是两者的相加的和。
### 50.知道css有个content属性吗？有什么作用？有什么应用？
    知道。css的content属性专门应用在 before/after 伪元素上，用来插入生成内容。最常见的应用是利用伪类清除浮动。
    //一种常见利用伪类清除浮动的代码
    .clearfix:after {
        content:"."; //这里利用到了content属性
        display:block; 
        height:0;
        visibility:hidden; 
        clear:both; }
    .clearfix { 
        *zoom:1; 
    }
    after伪元素通过 content 在元素的后面生成了内容为一个点的块级素，再利用clear:both清除浮动。
    　　那么问题继续还有，知道css计数器（序列数字字符自动递增）吗？如何通过css content属性实现css计数器？
    答案：css计数器是通过设置counter-reset 、counter-increment 两个属性 、及 counter()/counters()一个方法配合after / before 伪类实现。 
## js基础
### 1.eval是做什么的？
    它的功能是把对应的字符串解析成JS代码并运行；
    应该避免使用eval，不安全，非常耗性能（2次，一次解析成js语句，一次执行）。
    由JSON字符串转换为JSON对象的时候可以用eval，var obj =eval('('+ str +')');
### 2.什么是window对象? 什么是document对象?
    window对象是指浏览器打开的窗口。
    document对象是Documentd对象（HTML 文档对象）的一个只读引用，window对象的一个属性。
### 3.null，undefined 的区别？
    null      表示一个对象是“没有值”的值，也就是值为“空”；
    undefined     表示一个变量声明了没有初始化(赋值)；
    undefined不是一个有效的JSON，而null是；
    undefined的类型(typeof)是undefined；
    null的类型(typeof)是object；
    
    Javascript将未赋值的变量默认值设为undefined；
    Javascript从来不会将变量设为null。它是用来让程序员表明某个用var声明的变量时没有值的。
    typeof undefined
    //"undefined"
    undefined :是一个表示"无"的原始值或者说表示"缺少值"，就是此处应该有一个值，但是还没有定义。当尝试读取时会返回 undefined；
    例如变量被声明了，但没有赋值时，就等于undefined
    typeof null
    //"object"
    null : 是一个对象(空对象, 没有任何属性和方法)；
    例如作为函数的参数，表示该函数的参数不是对象；
    
    注意：
    在验证null时，一定要使用　=== ，因为 == 无法分别 null 和　undefined
    null == undefined // true
    null === undefined // false
    再来一个例子：
    null
    Q：有张三这个人么？
    A：有！
    Q：张三有房子么？
    A：没有！
    
    undefined
    Q：有张三这个人么？
    A：有！
    Q: 张三有多少岁？
    A: 不知道（没有被告诉）
### 4.["1", "2", "3"].map(parseInt) 答案是多少？
```javascript
 parseInt() 函数能解析一个字符串，并返回一个整数，需要两个参数 (val, radix)，
 其中 radix 表示要解析的数字的基数。【该值介于 2 ~ 36 之间，并且字符串中的数字不能大于radix才能正确返回数字结果值】;
 但此处 map 传了 3 个 (element, index, array),我们重写parseInt函数测试一下是否符合上面的规则。

 function parseInt(str, radix) {
     return str+'-'+radix;
 };
 var a=["1", "2", "3"];
 a.map(parseInt);  // ["1-0", "2-1", "3-2"] 不能大于radix

 因为二进制里面，没有数字3,导致出现超范围的radix赋值和不合法的进制解析，才会返回NaN
 所以["1", "2", "3"].map(parseInt) 答案也就是：[1, NaN, NaN]
```
### 5.事件是？IE与火狐的事件机制有什么区别？ 如何阻止冒泡？
    1. 我们在网页中的某个操作（有的操作对应多个事件）。例如：当我们点击一个按钮就会产生一个事件。是可以被 JavaScript 侦测到的行为。
    2. 事件处理机制：I.E.是事件冒泡、Firefox同时支持两种事件模型，也就是：捕获型事件和冒泡型事件；
    3. ev.stopPropagation();（旧ie的方法 ev.cancelBubble = true;）
### 6.javascript 代码中的"use strict";是什么意思 ? 使用它区别是什么？
     use strict是一种ECMAscript 5 添加的（严格）运行模式,这种模式使得 Javascript 在更严格的条件下运行,
    
     使JS编码更加规范化的模式,消除Javascript语法的一些不合理、不严谨之处，减少一些怪异行为。
     默认支持的糟糕特性都会被禁用，比如不能用with，也不能在意外的情况下给全局变量赋值;
     全局变量的显示声明,函数必须声明在顶层，不允许在非函数代码块内声明函数,arguments.callee也不允许使用；
     消除代码运行的一些不安全之处，保证代码运行的安全,限制函数中的arguments修改，严格模式下的eval函数的行为和非严格模式的也不相同;
    
     提高编译器效率，增加运行速度；
     为未来新版本的Javascript标准化做铺垫。
### 7.Javascript中，有一个函数，执行时对象查找时，永远不会去查找原型，这个函数是？
     hasOwnProperty
    
     javaScript中hasOwnProperty函数方法是返回一个布尔值，指出一个对象是否具有指定名称的属性。此方法无法检查该对象的原型链中是否具有该属性；该属性必须是对象本身的一个成员。
     使用方法：
     object.hasOwnProperty(proName)
     其中参数object是必选项。一个对象的实例。
     proName是必选项。一个属性名称的字符串值。
    
     如果 object 具有指定名称的属性，那么JavaScript中hasOwnProperty函数方法返回 true，反之则返回 false。
### 8.JSON 的了解？
```javascript
 JSON(JavaScript Object Notation) 是一种轻量级的数据交换格式。
 它是基于JavaScript的一个子集。数据格式简单, 易于读写, 占用带宽小
 如：{"age":"12", "name":"back"}

 JSON字符串转换为JSON对象:
 var obj =eval('('+ str +')');
 var obj = str.parseJSON();
 var obj = JSON.parse(str);

 JSON对象转换为JSON字符串：
 var last=obj.toJSONString();
 var last=JSON.stringify(obj);
```
### 9.js延迟加载的方式有哪些？
  defer和async、动态创建DOM方式（用得最多）、按需异步载入js
### 10.如何判断当前脚本运行在浏览器还是node环境中？（阿里）
    this === window ? 'browser' : 'node';
    通过判断Global对象是否为window，如果不为window，当前脚本没有运行在浏览器中
### 11.javascript的typeof返回哪些数据类型
```javascript
alert(typeof [1, 2]); //object
alert(typeof 'leipeng'); //string
var i = true; 
alert(typeof i); //boolean
alert(typeof 1); //number
var a; 
alert(typeof a); //undefined
function a(){;};
alert(typeof a) //function
```
### 12.例举3种强制类型转换和2种隐式类型转换?
    强制（parseInt(),parseFloat(),Number()）
    隐式（== ,!!）
### 13.split() 、join() 的区别
    前者是切割成数组的形式，后者是将数组转换成字符串
### 14.数组方法pop() push() unshift() shift()
    push()尾部添加 pop()尾部删除
    unshift()头部添加 shift()头部删除
    map() : 遍历数组中的元素, 返回一个新数组(包含回调函数返回的数据)
    filter():遍历数组中的元素, 返回一个新数组(包含回调函数返回true的元素)
### 15.事件绑定和普通事件有什么区别
```javascript
普通添加事件的方法：
var btn = document.getElementById("hello");
btn.onclick = function(){
    alert(1);
}
btn.onclick = function(){
    alert(2);
}
执行上面的代码只会alert 2 

事件绑定方式添加事件：
var btn = document.getElementById("hello");
btn.addEventListener("click",function(){
    alert(1);
},false);
btn.addEventListener("click",function(){
    alert(2);
},false);
执行上面的代码会先alert 1 再 alert 2
普通添加事件的方法不支持添加多个事件，最下面的事件会覆盖上面的，而事件绑定（addEventListener）方式添加事件可以添加多个。
addEventListener不兼容低版本IE
普通事件无法取消
addEventLisntener还支持事件冒泡+事件捕获
```
### 16.IE和DOM事件流的区别
    1.执行顺序不一样、
    2.参数不一样
    3.事件加不加on
    4.this指向问题
### 17.IE和标准下有哪些兼容性的写法
```javascript
var ev = ev || window.event
document.documentElement.clientWidth || document.body.clientWidth
var target = ev.srcElement||ev.target
```
### 18.如何阻止事件冒泡和事件默认行为
```javascript
//阻止事件冒泡
if(typeof ev.stopPropagation=='function') {  //标准的
    ev.stopPropagation();
} else { //非标准IE
    window.event.cancelBubble = true;
}
//阻止事件默认行为
return false
```
### 19.window.onload 和document ready的区别
    window.onload 是在dom文档树加载完和所有文件加载完之后执行一个函数document.ready原生中没有这个方法，jquery中有 $().ready(function),在dom文档树加载完之后执行一个函数（注意，这里面的文档树加载完不代表全部文件加载完）。
    $(document).ready要比window.onload先执行
    window.onload只能出来一次，$(document).ready可以出现多次
### 20.”==”和“===”的不同
    前者会自动转换类型
    后者不会
### 21.JavaScript是一门什么样的语言，它有哪些特点？
    javaScript一种直译式脚本语言，是一种动态类型、弱类型、基于原型的语言，内置支持类型。它的解释器被称为JavaScript引擎，为浏览器的一部分，广泛用于客户端的脚本语言，最早是在HTML网页上使用，用来给HTML网页增加动态功能。JavaScript兼容于ECMA标准，因此也称为ECMAScript。
    基本特点
    1. 是一种解释性脚本语言（代码不进行预编译）。
    2. 主要用来向HTML（标准通用标记语言下的一个应用）页面添加交互行为。
    3. 可以直接嵌入HTML页面，但写成单独的js文件有利于结构和行为的分离。
    跨平台特性，在绝大多数浏览器的支持下，可以在多种平台下运行（如Windows、Linux、Mac、Android、iOS等）。
### 22.JavaScript的数据类型都有什么？
```javascript
基本数据类型：String,boolean,Number,Undefined, Null
引用数据类型：Object, Array, Function
那么问题来了，如何判断某变量是否为数组数据类型？
方法一.判断其是否具有“数组性质”，如slice()方法。可自己给该变量定义slice方法，故有时会失效
方法二.obj instanceof Array 在某些IE版本中不正确
方法三.方法一二皆有漏洞，在ECMA Script5中定义了新方法Array.isArray(), 保证其兼容性，最好的方法如下：
    if(typeof Array.isArray==="undefined")
{
    Array.isArray = function(arg){
        return Object.prototype.toString.call(arg)==="[object Array]"
    };  
}
```
### 23.当一个DOM节点被点击时候，我们希望能够执行一个函数，应该怎么做？
    直接在DOM里绑定事件：<div onclick=”test()”></div>
    在JS里通过onclick绑定：xxx.onclick = test
    通过事件添加进行绑定：addEventListener(xxx, ‘click’, test)
    那么问题来了，Javascript的事件流模型都有什么？
    “事件冒泡”：事件开始由最具体的元素接受，然后逐级向上传播
    “事件捕捉”：事件由最不具体的节点先接收，然后逐级向下，一直到最具体的
    “DOM事件流”：三个阶段：事件捕捉，目标阶段，事件冒泡
### 24.看下列代码输出为何？解释原因。
    var a;
    alert(typeof a); // undefined
    alert(b); // 报错
    解释：Undefined是一个只有一个值的数据类型，这个值就是“undefined”，在使用var声明变量但并未对其赋值进行初始化时，这个变量的值就是undefined。而b由于未声明将报错。注意未申明的变量和声明了未赋值的是不一样的。
### 25.看下列代码,输出什么？解释原因。
```javascript
var undefined;
undefined == null; // true
1 == true;   // true
2 == true;   // false
0 == false;  // true
0 == '';     // true
NaN == NaN;  // false
[] == false; // true
[] == ![];   // true

undefined与null相等，但不恒等（===）
一个是number一个是string时，会尝试将string转换为number
尝试将boolean转换为number，0或1
尝试将Object转换成number或string，取决于另外一个对比量的类型
所以，对于0、空字符串的判断，建议使用 “===” 。“===”会先判断两边的值类型，类型不匹配时为false。
那么问题来了，看下面的代码，输出什么，foo的值为什么？
var foo = "11"+2-"1";
console.log(foo);
console.log(typeof foo);
执行完后foo的值为111，foo的类型为String。
```
### 26.已知数组var stringArray = [“This”, “is”, “Baidu”, “Campus”]，Alert出”This is Baidu Campus”。
    alert(stringArray.join(“”))
### 27.已知有字符串foo=”get-element-by-id”,写一个function将其转化成驼峰表示法”getElementById”
```javascript
function combo(msg){
    var arr=msg.split("-");
    for(var i=1;i<arr.length;i++){
     arr[i]=arr[i].charAt(0).toUpperCase()+arr[i]
.substr(1,arr[i].length-1);
    }
    msg=arr.join("");
    return msg;
}
```
### 28.输出今天的日期，以YYYY-MM-DD的方式，比如今天是2014年9月26日，则输出2014-09-26
```javascript
var d = new Date();
// 获取年，getFullYear()返回4位的数字
var year = d.getFullYear();
// 获取月，月份比较特殊，0是1月，11是12月
var month = d.getMonth() + 1;
var d = new Date();
// 获取年，getFullYear()返回4位的数字
var year = d.getFullYear();
// 获取月，月份比较特殊，0是1月，11是12月
var month = d.getMonth() + 1;
```
### 29.将字符串”<tr><td>{$id}</td><td>{$name}</td></tr>”中的{$id}替换成10，{$name}替换成Tony （使用正则表达式）
```javascript
"<tr><td>{$id}</td><td>{$id}_{$name}</td></tr>".replace(/{\$id}/g, '10').replace(/{\$name}/g, 'Tony');
```
### 30.为了保证页面输出安全，我们经常需要对一些特殊的字符进行转义，请写一个函数escapeHtml，将<, >, &, “进行转义
    function escapeHtml(str) {
    return str.replace(/[<>”&]/g, function(match) {
        switch (match) {
                       case “<”:
                          return “&lt;”;
                       case “>”:
                          return “&gt;”;
                       case “&”:
                          return “&amp;”;
                       case “\””:
                          return “&quot;”;
          }
      });
    }
### 31.foo = foo||bar ，这行代码是什么意思？为什么要这样写？
    如果foo存在，值不变，否则把bar的值赋给foo。
    短路表达式：作为”&&”和”||”操作符的操作数表达式，这些表达式在进行求值时，只要最终的结果已经可以确定是真或假，求值过程便告终止，这称之为短路求值。
### 32.看下列代码，将会输出什么?(变量声明提升)
```javascript
var foo = 1;
(function(){
    console.log(foo);
    var foo = 2;
    console.log(foo);
})()
答案：输出undefined 和 2。上面代码相当于：
var foo = 1;
(function(){
    var foo;
    console.log(foo); //undefined
    foo = 2;
    console.log(foo); // 2;   
})()
函数声明与变量声明会被JavaScript引擎隐式地提升到当前作用域的顶部，但是只提升名称不会提升赋值部分。
```
### 33.用js实现随机选取10–100之间的10个数字，存入一个数组，并排序。
```javascript
function randomNub(aArray, len, min, max) {
   if (len >= (max - min)) {
       return '超过' + min + '-' + max + '之间的个数范围' + (max - min - 1) + '个的总数';
   }
   if (aArray.length >= len) {
       aArray.sort(function(a, b) {
           return a - b
       });
       return aArray;
   }
   var nowNub = parseInt(Math.random() * (max - min - 1)) + (min + 1);
   for (var j = 0; j < aArray.length; j++) {
       if (nowNub == aArray[j]) {
           randomNub(aArray, len, min, max);
           return;
       }
   }
   aArray.push(nowNub);
   randomNub(aArray, len, min, max);
   return aArray;
     
     var arr=[];
     randomNub(arr,10,10,100);
```
### 34.把两个数组合并，并删除第二个元素。
    var array1 = ['a','b','c'];
    var bArray = ['d','e','f'];
    var cArray = array1.concat(bArray);
    cArray.splice(1,1);
### 35.有这样一个URL：http://item.taobao.com/item.htm?a=1&b=2&c=&d=xxx&e，请写一段JS程序提取URL中的各个GET参数(参数名和参数个数不确定)，将其按key-value形式返回到一个json结构中，如{a:’1′, b:’2′, c:”, d:’xxx’, e:undefined}。
```javascript
function serilizeUrl(url) {
  var urlObject = {};
  if (/\?/.test(url)) {
    var urlString = url.substring(url.indexOf("?") + 1);
    var urlArray = urlString.split("&");
    for (var i = 0, len = urlArray.length; i < len; i++) {
        var urlItem = urlArray[i];
        var item = urlItem.split("=");
        urlObject[item[0]] = item[1];
    }
    return urlObject;
  }
  return null;
}
```
### 36.正则表达式构造函数var reg=new RegExp(“xxx”)与正则表达字面量var reg=//有什么不同？匹配邮箱的正则表达式？
    当使用RegExp()构造函数的时候，不仅需要转义引号（即\”表示”），并且还需要双反斜杠（即\\表示一个\）。使用正则表达字面量的效率更高。 
    邮箱的正则匹配：
        var regMail = /^([a-zA-Z0-9_-])+@([a-zA-Z0-9_-])+((.[a-zA-Z0-9_-]{2,3}){1,2})$/;
### 37.写一个function，清除字符串前后的空格。
```javascript
if (!String.prototype.trim) { 
    String.prototype.trim = function() { 
    return this.replace(/^\s+/, "").replace(/\s+$/,"");
 } 
} 
//测试 
var str = " \t\n test string ".trim(); 
alert(str == "test string"); // alerts "true"
```
### 38.以下两个变量a和b，a+b的哪个结果是NaN？
```javascript
A、var a=undefined; b=NaN 
B、var a= ‘123’; b=NaN
C、var a =undefined , b =NaN
D、var a=NaN , b='undefined'
```
### 39.下面的JavaScript语句中，（ ）实现检索当前页面中的表单元素中的所有文本框，并将它们全部清空
```javascript
A. for(vari=0;i< form1.elements.length;i++) {
      if(form1.elements.type==”text”)
      form1.elements.value=”";
    }
B. for(vari=0;i<document.forms.length;i++) {
      if(forms[0].elements.type==”text”)
      forms[0].elements.value=”";
    }
C. if(document.form.elements.type==”text”)
  form.elements.value=”";
D. 
for(vari=0;i<document.forms.length; i++){
  for(var j=0;j<document.forms.elements.length; j++){
      if(document.forms.elements[j].type==”text”)
      document.forms.elements[j].value=”";
    }
}
```
### 40.typeof运算符返回值中有一个跟javascript数据类型不一致，它是
    Array
### 41.写出函数DateDemo的返回结果，系统时间假定为今天
```javascript
function DateDemo(){
 var d, s="今天日期是：";
 d = new Date();
s += d.getMonth() +1+ "/";
s += d.getDate() + "/";
s += d.getFullYear();
return s;
}
结果：今天日期是：7/21/2016
```
### 42.写出简单描述html标签（不带属性的开始标签和结束标签）的正则表达式，并将以下字符串中的html标签去除掉
```javascript
var str = “<div>这里是div<p>里面的段落</p></div>”;
<scripttype=”text/javascript”>
var reg = /<\/?\w+\/?>/gi;
var str = “<div>这里是div<p>里面的段落</p></div>”;
alert(str.replace(reg,”"));
</script>
```
### 43.截取字符串abcdefg的efg
  alert('abcdefg'.substring(4));
### 44.简述创建函数的几种方式
```javascript
第一种（函数声明）： 
function sum1(num1,num2){
   return num1+num2;
}
第二种（函数表达式）：
var sum2 = function(num1,num2){
   return num1+num2;
}
第三种（函数对象方式）：
var sum3 = new Function("num1","num2","return num1+num2");
```
### 45.Javascript如何实现继承？
    1.构造继承法
    2.原型继承法
    3.实例继承法
### 46.Javascript创建对象的几种方式？
```javascript
1、var obj = {};（使用json创建对象）
如：obj.name = '张三';
obj.action = function ()
{
alert('吃饭');
};
2、var obj = new Object();（使用Object创建对象）
如：obj.name = '张三';
obj.action = function ()
{
alert('吃饭');
};
3、通过构造函数创建对象。
(1)、使用this关键字
如：var obj = function (){
this.name ='张三';
this.age = 19;
this.action = function ()
{
alert('吃饭');
};
}
(2)、使用prototype关键字
如：function obj (){}
       obj.prototype.name ='张三';
obj.prototype.action=function ()
{
alert('吃饭');
};
4、使用内置对象创建对象。
如：var str = new String("实例初始化String");
var str1 = "直接赋值的String";
var func = new Function("x","alert(x)");//示例初始化func
var obj = new Object();//示例初始化一个Object
```
### 47.js延迟加载的方式有哪些？
    1. defer和async
    2. 动态创建DOM方式（创建script，插入到DOM中，加载完毕后callBack）
    3. 按需异步载入js
### 48.哪些操作会造成内存泄漏？
    内存泄漏指任何对象在您不再拥有或需要它之后仍然存在。
    垃圾回收器定期扫描对象，并计算引用了每个对象的其他对象的数量。如果一个对象的引用数量为 0（没有其他对象引用过该对象），或对该对象的惟一引用是循环的，那么该对象的内存即可回收。
    1. setTimeout 的第一个参数使用字符串而非函数的话，会引发内存泄漏。
    2. 闭包
    3. 控制台日志
    4. 循环（在两个对象彼此引用且彼此保留时，就会产生一个循环）
### 49.判断一个字符串中出现次数最多的字符，统计这个次数
```javascript
var str = 'asdfssaaasasasasaa';
var json = {};
for (var i = 0; i < str.length; i++) {
  if(!json[str.charAt(i)]){
          json[str.charAt(i)] = 1;
  }else{
          json[str.charAt(i)]++;
  }
};
var iMax = 0;
var iIndex = '';
for(var i in json){
  if(json[i]>iMax){
    iMax = json[i];
    iIndex = i;
  }
}
alert('出现次数最多的是:'+iIndex+'出现'+iMax+'次');
```
### 50.写一个获取非行间样式的函数
```javascript
function getStyle(obj,attr,value)
{
  if(!value)
  {
   if(obj.currentStyle)
   {
       return obj.currentStyle(attr);
   }
   else{
       obj.getComputedStyle(attr,false);
   }
  }   
  else
  {
   obj.style[attr] = value;
  }
}
```
### 51.字符串反转，如将 '12345678' 变成 '87654321'
    //思路：先将字符串转换为数组 split()，利用数组的反序函数 reverse()颠倒数组，再利用 jion() 转换为字符串
    var str = '12345678';
    str = str.split('').reverse().join('');
### 52.将数字 12345678 转化成 RMB形式 如： 12,345,678
```javascript
//思路：先将数字转为字符， str= str + '' ;
//利用反转函数，每三位字符加一个 ','最后一位不加； re()是自定义的反转函数，最后再反转回去！
function re(str) {
    str += '';
    return str.split("").reverse().join("");
}

function toRMB(num) {
    var tmp='';
    for (var  i  =  1;  i  <=  re(num).length;  i++) {    
        tmp  +=  re(num)[i  -  1];    
        if (i  %  3  ==  0  &&  i  !=  re(num).length) {        
            tmp  +=  ',';    
        }
    }
    return re(tmp);
}
```
### 53.生成5个不同的随机数；
```javascript
//思路：5个不同的数，每生成一次就和前面的所有数字相比较，如果有相同的，则放弃当前生成的数字！
var num1 = [];
for(var i = 0; i < 5; i++){
    num1[i] = Math.floor(Math.random()*10) + 1; //范围是 [1, 10]
    for(var j = 0; j < i; j++){
        if(num1[i] == num1[j]){
            i--;
        }
    }
}
```
### 54.去掉数组中重复的数字
```javascript
方法一:
//思路：每遍历一次就和之前的所有做比较，不相等则放入新的数组中！
//这里用的原型 个人做法；
Array.prototype.unique = function(){
    var len = this.length,
        newArr = [],
        flag = 1;
    for(var i = 0; i < len; i++, flag = 1){
        for(var j = 0; j < i; j++){
            if(this[i] == this[j]){
                flag = 0;        //找到相同的数字后，不执行添加数据
            }
        }
        flag ? newArr.push(this[i]) : '';
    }
    return newArr;
}
方法二：
var arr=[1,2,3,3,4,4,5,5,6,1,9,3,25,4];
Array.prototype.unique2 = function()
{
    var n = []; //一个新的临时数组
    for(var i = 0; i < this.length; i++) //遍历当前数组
    {
         //如果当前数组的第i已经保存进了临时数组，那么跳过，
         //否则把当前项push到临时数组里面
         if (n.indexOf(this[i]) == -1) n.push(this[i]);
    }
    return n;
}
var newArr2=arr.unique2(arr);
alert(newArr2); //输出1,2,3,4,5,6,9,25
```
### 55.阶乘函数；
```javascript
//原型方法
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
### 56.window.location.search() 返回的是什么？
    http://localhost:8080/xxx?ver=1.0&id=123
    返回值：?ver=1.0&id=timlq 也就是问号后面的部分
### 57.window.location.reload() 作用？
    新当前页面。
### 58.javascript 中的垃圾回收机制？
    在Javascript中，如果一个对象不再被引用，那么这个对象就会被GC回收。如果两个对象互相引用，而不再  被第3者所引用，那么这两个互相引用的对象也会被回收。因为函数a被b引用，b又被a外的c引用，这就是为什么  函数a执行后不会被回收的原因。
### 59.精度问题: JS 精度不能精确到 0.1 所以  。。。。同时存在于值和差值中
```javascript
var n = 0.3,m = 0.2, i = 0.2, j = 0.1;
alert((n - m) == (i - j)); //false
alert((n-m) == 0.1); //false
alert((i-j)==0.1); //true
```
### 60.计算字符串字节数：
```javascript
new function(s){ 
   if(!arguments.length||!s) return null;  
   if(""==s) return 0;     
   var l=0;
   for(var i=0;i<s.length;i++){        
       if(s.charCodeAt(i)>255) l+=2; else l+=1;  //charCodeAt()得到的是unCode码   
   }     //汉字的unCode码大于 255bit 就是两个字节
   alert(l); 
}("hello world!");
```
### 61.匹配输入的字符：第一个必须是字母或下划线开头，长度5-20
```javascript
var reg = /^[a-zA-Z_][a-zA-Z0-9_]{5,20}/,
  name1 = 'leipeng',
  name2 = '0leipeng',
  name3 = '你好leipeng',
  name4 = 'hi';
alert(reg.test(name1));
alert(reg.test(name2));
alert(reg.test(name3));
alert(reg.test(name4));
```
### 62.如何在HTML中添加事件，几种方法？
```javascript
1、标签之中直接添加 onclick="fun()";
2、JS添加 Eobj.onclick = method;
3、绑定事件  IE： obj.attachEvent('onclick', method)；
             FF: obj.addEventListener('click', method, false);
```
### 63.你如何优化自己的代码？
    代码重用
    避免全局变量（命名空间，封闭空间，模块化mvc..）
    拆分函数避免函数过于臃肿
    注释
### 64.使用js实现这样的效果：在文本域里输入文字时，当按下enter键时不换行，而是替换成“{{enter}}”,(只需要考虑在行尾按下enter键的情况).
```javascript
<html>
<head>
    <script>
        function back(ele,event){
            event = event || window.event;
            if(event.keyCode==13){
                event.returnValue = false;
                ele.value+="{{enter}}"
                return false;
            }
        }
    </script>
</head>
<body>
<textarea rows="3" cols="40" id="te" onkeypress="back(this,event);"></textarea>
</body>
</html>
```
### 65.简述readyonly与disabled的区别
    ReadOnly和Disabled的作用是使用户不能够更改表单域中的内容.
    但是二者还是有着一些区别的：
    1、Readonly只针对input(text/password)和textarea有效，而disabled对于所有的表单元素有效，包括select,radio,checkbox,button等。
    2、在表单元素使用了disabled后，我们将表单以POST或者GET的方式提交的话，这个元素的值不会被传递出去，而readonly会将该值传递出去
### 66.为什么扩展javascript内置对象不是好的做法？
    因为你不知道哪一天浏览器或javascript本身就会实现这个方法，而且和你扩展的实现有不一致的表现。到时候你的javascript代码可能已经在无数个页面中执行了数年，而浏览器的实现导致所有使用扩展原型的代码都崩溃了。。。
### 67.什么是三元表达式？“三元”表示什么意思？
    三元运算符:
    三元如名字表示的三元运算符需要三个操作数。
    语法是 条件 ? 结果1 : 结果2;. 
    这里你把条件写在问号(?)的前面后面跟着用冒号(:)分隔的结果1和结果2。满足条件时结果1否则结果2。
### 68.变量的命名规范以及命名推荐
    变量，函数，方法：小写开头，以后的每个单词首字母大写 （驼峰）
    构造函数，class：每个单词大写开头
    基于实际情况，以动词，名词，谓词来命名。尽量言简意骇，以命名代替注释
### 69.三种弹窗的单词以及三种弹窗的功能
```javascript
1.alert  
//弹出对话框并输出一段提示信息  
    function ale() {  
        //弹出一个对话框  
        alert("提示信息！");  
    }  
2.confirm
    //弹出一个询问框，有确定和取消按钮  
    function firm() {  
        //利用对话框返回的值 （true 或者 false）  
        if (confirm("你确定提交吗？")) {  
            alert("点击了确定");  
        }  
        else {  
            alert("点击了取消");  
        }  
    }  
3.prompt
    //弹出一个输入框，输入一段文字，可以提交  
    function prom() {  
        var name = prompt("请输入您的名字", ""); //将输入的内容赋给变量 name ，  

        //这里需要注意的是，prompt有两个参数，前面是提示的话，后面是当对话框出来后，在对话框里的默认值  
        if (name)//如果返回的有内容  
        {  
            alert("欢迎您：" + name)  
        }  
    }
```
### 70.主流浏览器内核
    IE trident        火狐gecko    谷歌苹果webkit    Opera：Presto
    71.JavaScript的循环语句有哪些？
    for,for..in,while,do...while
### 72.闭包：下面这个ul，如何点击每一列的时候alert其index？
```javascript
<ul id="test">
    <li>这是第一条</li>
    <li>这是第二条</li>
    <li>这是第三条</li>
</ul>
//js
 window.onload = function() {
        var lis = document.getElementById('test').children;
        for (var i = 0; i < lis.length; i++) {
            lis[i].onclick = (function(i) {
                return function() {
                    alert(i)
                };
            })(i);
        };
    }
```
### 73.列出3条以上ff和IE的脚本兼容问题
```javascript
(1) window.event： 
表示当前的事件对象，IE有这个对象，FF没有，FF通过给事件处理函数传递事件对象 

(2) 获取事件源 
IE用srcElement获取事件源，而FF用target获取事件源 

(3) 添加，去除事件 
IE：element.attachEvent(“onclick”, function) element.detachEvent(“onclick”, function) 
FF：element.addEventListener(“click”, function, true) element.removeEventListener(“click”, function, true) 

(4) 获取标签的自定义属性 
IE：div1.value或div1[“value”] 
FF：可用div1.getAttribute(“value”)
```
### 74.在Javascript中什么是伪数组？如何将伪数组转化为标准数组？
    伪数组（类数组）：无法直接调用数组方法或期望length属性有什么特殊的行为，但仍可以对真正数组遍历方法来遍历它们。典型的是函数的argument参数，还有像调用getElementsByTagName,document.childNodes之类的,它们都返回NodeList对象都属于伪数组。可以使用Array.prototype.slice.call(fakeArray)将数组转化为真正的Array对象。
### 75.请写出一个程序，在页面加载完成后动态创建一个form表单，并在里面添加一个input对象并给它任意赋值后义post方式提交到：http://127.0.0.1/save.php
```javascript
window.onload=function(){
    var form=document.createElement("form");
    form.setAttribute("method", "post");
    form.setAttribute("action", "http://127.0.0.1/save.php");
    
    var input=document.createElement("input");
    form.appendChild(input);
    document.body.appendChild(form);
    input.value="cxc";
    form.submit();//提交表单
}
```
### 76.用JavaScript实现冒泡排序。数据为23、45、18、37、92、13、24
```javascript
//升序算法
function sort(arr){
    for (var i = 0; i <arr.length; i++) {
        for (var j = 0; j <arr.length-i; j++) {
            if(arr[j]>arr[j+1]){
                var c=arr[j];//交换两个变量的位置
                arr[j]=arr[j+1];
                arr[j+1]=c;
            }
        };
    };
    return arr.toString();
}
console.log(sort([23,45,18,37,92,13,24]));
```
### 77.前端代码优化的方法
```javascript
var User = { 
    count = 1，
    getCount：function（）{ 
         return this.count;
    }
}
console.log(User.getCount());
var func = User.getCount;
console.log(func());
1 undefined（因为是window对象执行了func函数）;
```
### 78.用程序实现找到html中id名相同的元素？
```javascript
<body>
    <form id='form1'>
        <div id='div1'></div>
        <div id='div2'></div>
        <div id='div3'></div>
        <div id='div4'></div>
        <div id='div5'></div>
        <div id='div3'>id名重复的元素</div>
    </form>
</body>
var nodes=document.querySelectorAll("#form1>*");
for(var i=0,len=nodes.length;i<len;i++){
    var attr=nodes[i].getAttribute("id");
    var s=1;
    for(var j=i+1;j<len;j++){
        if(nodes[j].getAttribute("id")==attr){
            s++;
            alert("id为："+attr+"的元素出现"+s+"次");
        }
    }
}
```
### 79.程序中捕获异常的方法？
```javascript
window.error
try{}catch(){}finally{}
```
### 80.下列控制台都输出什么
```javascript
第1题：
function setName(){
    name="张三";
}
setName();
console.log(name);
答案："张三"
第2题：
//考点：1、变量声明提升 2、变量搜索机制
var a=1;
function test(){
    console.log(a);
    var a=1;
}
test();
答案：undefined
第3题：
var b=2;
function test2(){
    window.b=3;
    console.log(b);
}
test2();
答案：3
第4题：
c=5;//声明一个全局变量c 
function test3(){
    window.c=3;
    console.log(c);        //答案：undefined，原因：由于此时的c是一个局部变量c，并且没有被赋值
    var c;
    console.log(window.c);//答案：3，原因：这里的c就是一个全局变量c
}
test3();
第5题：
var arr = [];
arr[0]  = 'a';
arr[1]  = 'b';
arr[10] = 'c';
alert(arr.length); //答案：11
console.log(arr[5]); //答案：undefined
第6题：
var a=1;
console.log(a++);        //答案：1
console.log(++a);        //答案：3
第7题：
console.log(null==undefined); //答案：true
console.log("1"==1);       //答案：true，因为会将数字1先转换为字符串1
console.log("1"===1);     //答案：false，因为数据类型不一致
第8题：
typeof 1;        "number"
typeof "hello";        "string"
typeof /[0-9]/;        "object"
typeof {};        "object"
typeof null;        "object"
typeof undefined;   "undefined"
typeof [1,2,3];     "object"
typeof function(){}; //"function"
第9题：
parseInt(3.14);             //3
parseFloat("3asdf");         //3
parseInt("1.23abc456");
parseInt(true);//"true" NaN
第10题：
//考点：函数声明提前
function bar() {
    return foo;
    foo = 10;
    function foo() {}
    //var foo = 11;
}
alert(typeof bar());//"function"
第11题： 
//考点：函数声明提前
var foo = 1;
function bar() {
    foo = 10;
    return;
    function foo() {}
}
bar();
alert(foo);//答案：1
第12题：
console.log(a);//是一个函数
var a = 3;
function a(){}
console.log(a);////3
第13题：
//考点：对arguments的操作
function foo(a) {
    arguments[0] = 2;
    alert(a);//答案：2，因为：a、arguments是对实参的访问，b、通过arguments[i]可以修改指定实参的值
}
foo(1);
第14题：
function foo(a) {
    alert(arguments.length);//答案：3，因为arguments是对实参的访问
}
foo(1, 2, 3);
第15题
bar();//报错
var foo = function bar(name) {
    console.log("hello"+name);
    console.log(bar);
};
//alert(typeof bar);
foo("world");//"hello"
console.log(bar);//undefined
console.log(foo.toString());
bar();//报错
第16题： 
function test(){
    console.log("test函数");
}
setTimeout(function(){
    console.log("定时器回调函数");
}, 0)
test();
结果：
test函数
定时器回调函数
```
### 81.请说出三种减低页面加载时间的方法
    1、压缩css、js文件
    2、合并js、css文件，减少http请求
    3、外部js、css文件放在最底下
    4、减少dom操作，尽可能用变量替代不必要的dom操作
### 82.实现一个函数clone，可以对JavaScript中的5种主要的数据类型（包括Number、String、Object、Array、Boolean）进行值复制
```javascript
// 方法一：
Object.prototype.clone = function(){
   var o = this.constructor === Array ? [] : {};
   for(var e in this){
      o[e] = typeof this[e] === "object" ? this[e].clone() : this[e];
   }
   return o;
}
//方法二：
 /**
 * 克隆一个对象
 * @param Obj
 * @returns
 */
function clone(Obj) {   
    var buf;   
    if (Obj instanceof Array) {   
        buf = [];//创建一个空的数组 
        var i = Obj.length;   
        while (i--) {   
            buf[i] = clone(Obj[i]);   
        }   
        return buf;    
    }else if (Obj instanceof Object){   
        buf = {};//创建一个空对象 
        for (var k in Obj) { //为这个对象添加新的属性 
            buf[k] = clone(Obj[k]);   
        }   
        return buf;   
    }else{ //普通变量直接赋值
        return Obj;   
    }   
}
```
### 83.如何消除一个数组里面重复的元素？
```javascript
var arr=[1,2,3,3,4,4,5,5,6,1,9,3,25,4];
function deRepeat(){
    var newArr=[];
    var obj={};
    var index=0;
    var l=arr.length;
    for(var i=0;i<l;i++){
        if(obj[arr[i]]==undefined)
          {
            obj[arr[i]]=1;
            newArr[index++]=arr[i];
          }
        else if(obj[arr[i]]==1)
          continue;
    }
    return newArr;
}
var newArr2=deRepeat(arr);
alert(newArr2); //输出1,2,3,4,5,6,9,25
```
### 84.定义一个log方法，让它可以代理console.log的方法。
```javascript
function log(msg)　{
    console.log(msg);
}
log("hello world!") // hello world!
如果要传入多个参数呢？显然上面的方法不能满足要求，所以更好的方法是：
function log(){
    console.log.apply(console, arguments);
};
到此，追问apply和call方法的异同。
对于apply和call两者在作用上是相同的，即是调用一个对象的一个方法，以另一个对象替换当前对象。将一个函数的对象上下文从初始的上下文改变为由 thisObj 指定的新对象。
但两者在参数上有区别的。对于第一个参数意义都一样，但对第二个参数： apply传入的是一个参数数组，也就是将多个参数组合成为一个数组传入，而call则作为call的参数传入（从第二个参数开始）。 如 func.call(func1,var1,var2,var3)对应的apply写法为：func.apply(func1,[var1,var2,var3]) 。
```
### 85.原生JS的window.onload与Jquery的$(document).ready(function(){})有什么不同？如何用原生JS实现Jq的ready方法？
```javascript
window.onload()方法是必须等到页面内包括图片的所有元素加载完毕后才能执行。
$(document).ready()是DOM结构绘制完毕后就执行，不必等到加载完毕。
/*
 * 传递函数给whenReady()
 * 当文档解析完毕且为操作准备就绪时，函数作为document的方法调用
 */
var whenReady = (function() {               //这个函数返回whenReady()函数
    var funcs = [];             //当获得事件时，要运行的函数
    var ready = false;          //当触发事件处理程序时,切换为true
    //当文档就绪时,调用事件处理程序
    function handler(e) {
        if(ready) return;       //确保事件处理程序只完整运行一次
        //如果发生onreadystatechange事件，但其状态不是complete的话,那么文档尚未准备好
        if(e.type === 'onreadystatechange' && document.readyState !== 'complete') {
            return;
        }
        //运行所有注册函数
        //注意每次都要计算funcs.length
        //以防这些函数的调用可能会导致注册更多的函数
        for(var i=0; i<funcs.length; i++) {
            funcs[i].call(document);
        }
        //事件处理函数完整执行,切换ready状态, 并移除所有函数
        ready = true;
        funcs = null;
    }
    //为接收到的任何事件注册处理程序
    if(document.addEventListener) {
        document.addEventListener('DOMContentLoaded', handler, false);
        document.addEventListener('readystatechange', handler, false);            //IE9+
        window.addEventListener('load', handler, false);
    }else if(document.attachEvent) {
        document.attachEvent('onreadystatechange', handler);
        window.attachEvent('onload', handler);
    }
    //返回whenReady()函数
    return function whenReady(fn) {
        if(ready) { fn.call(document); }
        else { funcs.push(fn); }
    }
})();
如果上述代码十分难懂，下面这个简化版：
function ready(fn){
    if(document.addEventListener) {//标准浏览器
        document.addEventListener('DOMContentLoaded', function() {
            //注销事件, 避免反复触发
            document.removeEventListener('DOMContentLoaded',arguments.callee, false);
            fn();//执行函数
        }, false);
    }else if(document.attachEvent) {//IE
        document.attachEvent('onreadystatechange', function() {
            if(document.readyState == 'complete') {
                document.detachEvent('onreadystatechange', arguments.callee);
                fn();//函数执行
            }
        });
    }
};
```
### 86.（设计题）想实现一个对页面某个节点的拖曳？如何做？（使用原生JS）
    回答出概念即可，下面是几个要点
    给需要拖拽的节点绑定mousedown, mousemove, mouseup事件
    mousedown事件触发后，开始拖拽
    mousemove时，需要通过event.clientX和clientY获取拖拽位置，并实时更新位置
    mouseup时，拖拽结束
    需要注意浏览器边界的情况
### 87.数组和对象有哪些原生方法，列举一下？
```javascript
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

Object.hasOwnProperty( ) 检查属性是否被继承 
Object.isPrototypeOf( ) 一个对象是否是另一个对象的原型 
Object.propertyIsEnumerable( ) 是否可以通过for/in循环看到属性 
Object.toLocaleString( ) 返回对象的本地字符串表示 
Object.toString( ) 定义一个对象的字符串表示 
Object.valueOf( ) 指定对象的原始值
```
### 88.JS 怎么实现一个类。怎么实例化这个类
  严格来讲js中并没有类的概念，不过js中的函数可以作为构造函数来使用，通过new来实例化，其实函数本身也是一个对象。
### 89.在JS中有哪些会被隐式转换为false
  Undefined、null、关键字false、NaN、零、空字符串
### 90.外部JS文件出现中文字符，会出现什么问题，怎么解决？
  会出现乱码，加charset=”utf-8”;
### 91.写一个通用的事件侦听器函数
    // event(事件)工具集，来源：https://github.com/markyun
    markyun.Event = {
        // 页面加载完成后
        readyEvent : function(fn) {
            if (fn==null) {
                fn=document;
            }
            var oldonload = window.onload;
            if (typeof window.onload != 'function') {
                window.onload = fn;
            } else {
                window.onload = function() {
                    oldonload();
                    fn();
                };
            }
        },
        // 视能力分别使用dom0||dom2||IE方式 来绑定事件
        // 参数： 操作的元素,事件名称 ,事件处理程序
        addEvent : function(element, type, handler) {
            if (element.addEventListener) {
                //事件类型、需要执行的函数、是否捕捉
                element.addEventListener(type, handler, false);
            } else if (element.attachEvent) {
                element.attachEvent('on' + type, function() {
                    handler.call(element);
                });
            } else {
                element['on' + type] = handler;
            }
        },
        // 移除事件
        removeEvent : function(element, type, handler) {
            if (element.removeEnentListener) {
                element.removeEnentListener(type, handler, false);
            } else if (element.datachEvent) {
                element.detachEvent('on' + type, handler);
            } else {
                element['on' + type] = null;
            }
        }, 
        // 阻止事件 (主要是事件冒泡，因为IE不支持事件捕获)
        stopPropagation : function(ev) {
            if (ev.stopPropagation) {
                ev.stopPropagation();
            } else {
                ev.cancelBubble = true;
            }
        },
        // 取消事件的默认行为
        preventDefault : function(event) {
            if (event.preventDefault) {
                event.preventDefault();
            } else {
                event.returnValue = false;
            }
        },
        // 获取事件目标
        getTarget : function(event) {
            return event.target || event.srcElement;
        },
        // 获取event对象的引用，取到事件的所有信息，确保随时能使用event；
        getEvent : function(e) {
            var ev = e || window.event;
            if (!ev) {
                var c = this.getEvent.caller;
                while (c) {
                    ev = c.arguments[0];
                    if (ev && Event == ev.constructor) {
                        break;
                    }
                    c = c.caller;
                }
            }
            return ev;
        }
    }; 
### 92.JSON 的了解
    JSON(JavaScript Object Notation) 是一种轻量级的数据交换格式。它是基于JavaScript的一个子集。数据格式简单, 易于读写, 占用带宽小
    {'age':'12', 'name':'back'}
## JS高级 --- DOM相关
### 1.原生DOM操作和事件
    1. 如需替换 HTML DOM 中的元素，请使用replaceChild(newnode,oldnode)方法
    2. 从父元素中删除子元素 parent.removeChild(child);
    3. insertBefore(newItem,existingItem) 在指定的已有子节点之前插入新的子节点
    4. appendChild(newListItem向元素添加新的子节点，作为最后一个子节点 
    5. document.documentElement - 全部文档 document.body - 文档的主体
       http://www.w3school.com.cn/jsref/dom_obj_all.asp

### 2.JS事件：target与currentTarget区别
    target在事件流的目标阶段；
    currentTarget在事件流的捕获，目标及冒泡阶段。
    只有当事件流处在目标阶段的时候，两个的指向才是一样的， 而当处于捕获和冒泡阶段的时候，target指向被单击的对象而currentTarget指向当前事件活动的对象（一般为父级）。

### 3.事件模型
    事件捕捉阶段：事件开始由顶层对象触发，然后逐级向下传播，直到目标的元素； 
    处于目标阶段：处在绑定事件的元素上； 
    事件冒泡阶段：事件由具体的元素先接收，然后逐级向上传播，直到不具体的元素；
    阻止 冒泡／捕获 event.stopPropagation()和IE的event.cancelBubble=true

### 4.DOM事件绑定 
    1.绑定事件监听函数：addEventListener和attchEvent 
    2.在JavaScript代码中绑定：获取DOM元素 dom.onlick = fn 
    3.在DOM元素中直接绑定：<div onclick = 'fn()'>
### 5.DOM事件流包括三个阶段：
    事件捕获阶段、处于目标阶段、事件冒泡阶段。
    首先发生的事件捕获，为截获事件提供机会。
    然后是实际的目标接受事件。
    最后一个阶段是时间冒泡阶段，可以在这个阶段对事件做出响应。

### 6.什么是事件委托
    因为事件具有冒泡机制，因此我们可以利用冒泡的原理，把事件加到父级上，触发执行效果。这样做的好处当然就是提高性能了
    最重要的是通过event.target.nodeName判断子元素
    <div>
        <ul id = "bubble">
            <li>1</li>
            <li>2</li>
            <li>3</li>
            <li>4</li>
        </ul>
    </div>
    window.onload = function () {
        var aUl = document.getElementsById("bubble");
        var aLi = aUl.getElementsByTagName("li");
    
        //不管在哪个事件中，只要你操作的那个元素就是事件源。
        // ie：window.event.srcElement
        // 标准下:event.target
        aUl.onmouseover = function (ev) {
            var ev = ev || window.event;
            var target = ev.target || ev.srcElement;
     
            if(target.nodeName.toLowerCase() == "li"){
                target.style.background = "blue";
            }
        };
    };
### 7.documen.write和 innerHTML的区别
    document.write只能重绘整个页面
    innerHTML可以重绘页面的一部分
### 8.DOM操作——怎样添加、移除、移动、复制、创建和查找节点?
    （1）创建新节点
      createDocumentFragment()    //创建一个DOM片段
      createElement()   //创建一个具体的元素
      createTextNode()   //创建一个文本节点
    （2）添加、移除、替换、插入
      appendChild()
      removeChild()
      replaceChild()
      insertBefore() //在已有的子节点前插入一个新的子节点
    （3）查找
      getElementsByTagName()    //通过标签名称
      getElementsByName()    //通过元素的Name属性的值(IE容错能力较强，会得到一个数组，其中包括id等于name值的)
      getElementById()    //通过元素Id，唯一性
## JS高级
### 1.闭包
    特性： 1.函数嵌套函数 2.函数内部可以引用外部的参数和变量 3.参数和变量不会被垃圾回收机制回收
    闭包的缺点就是常驻内存，会增大内存使用量，使用不当很容易造成内存泄露。
    为什么要使用闭包：
    为了设计私有方法和变量，避免全局变量污染 希望一个变量长期驻扎在内存中
    view detail: https://segmentfault.com/a/1190000000652891
### 2.原型与原型链
    当从一个对象那里调取属性或方法时，如果该对象自身不存在这样的属性或方法，就会去自己关联的prototype对象那里寻找，如果prototype没有，就会去prototype关联的前辈prototype那里寻找，如果再没有则继续查找Prototype.Prototype引用的对象，依次类推，直到Prototype.….Prototype为undefined（Object的Prototype就是undefined）从而形成了所谓的“原型链”。
    其中foo是Function对象的实例。而Function的原型对象同时又是Object的实例。这样就构成了一条原型链。
    instanceof 确定原型和实例之间的关系
    用来判断某个构造函数的prototype属性是否存在另外一个要检测对象的原型链上
    对象的__proto__指向自己构造函数的prototype。obj.__proto__.__proto__...的原型链由此产生，包括我们的操作符instanceof正是通过探测obj.__proto__.__proto__... === Constructor.prototype来验证obj是否是Constructor的实例。
    function C(){}
    var o = new C(){}
    //true 因为Object.getPrototypeOf(o) === C.prototype
    o instanceof C
    instanceof只能用来判断对象和函数，不能用来判断字符串和数字
    isPrototypeOf
    用于测试一个对象是否存在于另一个对象的原型链上。
    判断父级对象 可检查整个原型链
### 3.作用域与作用域链
    作用域链的作用是保证执行环境里有权访问的变量和函数是有序的，作用域链的变量只能向上访问，变量访问到window对象即被终止，作用域链向下访问变量是不被允许的。
    全局函数无法查看局部函数的内部细节，但局部函数可以查看其上层的函数细节，直至全局细节。
    当需要从局部函数查找某一属性或方法时，如果当前作用域没有找到，就会上溯到上层作用域查找，
    直至全局函数，这种组织形式就是作用域链。
### 4.apply, call和bind有什么区别?
    参考答案：三者都可以把一个函数应用到其他对象上，call、apply是修改函数的作用域（修改this指向），并且立即执行，而bind是返回了一个新的函数，不是立即执行．apply和call的区别是apply接受数组作为参数，而call是接受逗号分隔的无限多个参数列表，
    
    Array.prototype.slice.call(null, args)
    
    function getMax(arr){
      return Math.max.apply(null, arr);
    }
    //call
    function foo() {
      console.log(this);//{id: 42}
    }
    
    foo.call({ id: 42 });
    如果该方法是非严格模式代码中的函数，则null和undefined将替换为全局对象，并且原始值将被包装。 当你调用apply传递给它null时，就像是调用函数而不提供任何对象
### 5.谈谈对this的理解
    this总是指向函数的直接调用者（而非间接调用者）；
    如果有new关键字，this指向new出来的那个对象；
    在事件中，this指向触发这个事件的对象，特殊的是，IE中的attachEvent中的this总是指向全局对象Window；
### 6.那些操作会造成内存泄漏？
    内存泄漏指任何对象在您不再拥有或需要它之后仍然存在。
    垃圾回收器定期扫描对象，并计算引用了每个对象的其他对象的数量。如果一个对象的引用数量为 0（没有其他对象引用过该对象），或对该对象的惟一引用是循环的，那么该对象的内存即可回收。
    setTimeout 的第一个参数使用字符串而非函数的话，会引发内存泄漏。
    闭包、控制台日志、循环（在两个对象彼此引用且彼此保留时，就会产生一个循环）
### 7.深入贯彻闭包思想，全面理解JS闭包形成过程
    https://segmentfault.com/a/1190000009886713
### 8.下面这个ul，如何点击每一列的时候alert其index?（闭包）
    <ul id=”test”>
    <li>这是第一条</li>
    <li>这是第二条</li>
    <li>这是第三条</li>
    </ul>
    // 方法一：
    var lis=document.getElementById('2223').getElementsByTagName('li');
    for(var i=0;i<3;i++)
    {
        lis[i].index=i;
        lis[i].onclick=function(){
            alert(this.index);
        };
    }
    //方法二：
    var lis=document.getElementById('2223').getElementsByTagName('li');
    for(var i=0;i<3;i++){
        lis[i].index=i;
        lis[i].onclick=(function(a){
            return function() {
                alert(a);
            }
        })(i);
    }
### 9.js继承方式及其优缺点
    原型链继承的缺点
    一 是字面量重写原型会中断关系，使用引用类型的原型，并且子类型还无法给超类型传递参数。
    借用构造函数（类式继承）
    借用构造函数虽然解决了刚才两种问题，但没有原型，则复用无从谈起。所以我们需要原型链+借用构造函数的模式，这种模式称为组合继承
    
    组合式继承
    组合式继承是比较常用的一种继承方法，其背后的思路是 使用原型链实现对原型属性和方法的继承，而通过借用构造函数来实现对实例属性的继承。这样，既通过在原型上定义方法实现了函数复用，又保证每个实例都有它自己的属性。
### 10.什么是上下文环境对象
### 11.new操作符具体做了什么
    1、创建一个空对象，并且this变量引用该对象，同时继承了该函数的原型（实例对象通过__proto__属性指向原型对象；obj.__proto__ = Base.prototype;） 2、属性和方法被加入到 this 引用的对象中。
    
    function Animal(name) {
        this.name = name;
    }
    
    Animal.prototype.run = function() {
        console.log(this.name + 'can run...');
    }
    
    var cat = new Animal('cat');
    //模拟过程
    new Animal('cat')=function(){
        let obj={};  //创建一个空对象
        obj.__proto__=Animal.prototype;
        //把该对象的原型指向构造函数的原型对象，就建立起原型了：obj->Animal.prototype->Object.prototype->null
        return Animal.call(obj,'cat');// 绑定this到实例化的对象上
    }
### 12.创建对象的几种方式
    javascript创建对象简单的说,无非就是使用内置对象或各种自定义对象，当然还可以用JSON；但写法有很多种，也能混合使用。
    1、对象字面量的方式
      person={firstname:"Mark",lastname:"Yun",age:25,eyecolor:"black"};
    2、用function来模拟无参的构造函数
      function Person(){}
      var person=new Person();//定义一个function，如果使用new"实例化",该function可以看作是一个Class
      person.name="Mark";
      person.age="25";
      person.work=function(){
      alert(person.name+" hello...");
      }
      person.work();
    3、用function来模拟参构造函数来实现（用this关键字定义构造的上下文属性）
    
      function Pet(name,age,hobby){
         this.name=name;//this作用域：当前对象
         this.age=age;
         this.hobby=hobby;
         this.eat=function(){
            alert("我叫"+this.name+",我喜欢"+this.hobby+",是个程序员");
         }
      }
      var maidou =new Pet("麦兜",25,"coding");//实例化、创建对象
      maidou.eat();//调用eat方法
    4、用工厂方式来创建（内置对象）
    
       var wcDog =new Object();
       wcDog.name="旺财";
       wcDog.age=3;
       wcDog.work=function(){
         alert("我是"+wcDog.name+",汪汪汪......");
       }
       wcDog.work();
    
    5、用原型方式来创建
      function Dog(){
       
       }
       Dog.prototype.name="旺财";
       Dog.prototype.eat=function(){
       alert(this.name+"是个吃货");
       }
       var wangcai =new Dog();
       wangcai.eat();
    
    5、用混合方式来创建
      function Car(name,price){
        this.name=name;
        this.price=price;
      }
       Car.prototype.sell=function(){
         alert("我是"+this.name+"，我现在卖"+this.price+"万元");
        }
      var camry =new Car("凯美瑞",27);
      camry.sell();
### 13.什么是线程机制
    1.同步和异步的区别?
    同步：阻塞的
      -张三叫李四去吃饭，李四一直忙得不停，张三一直等着，直到李四忙完两个人一块去吃饭
      =浏览器向服务器请求数据，服务器比较忙，浏览器一直等着（页面白屏），直到服务器返回数据，浏览器才能显示页面
    异步：非阻塞的
      -张三叫李四去吃饭，李四在忙，张三说了一声然后自己就去吃饭了，李四忙完后自己去吃
      =浏览器向服务器请求数据，服务器比较忙，浏览器可以自如的干原来的事情（显示页面），服务器返回数据的时候通知浏览器一声，浏览器把返回的数据再渲染到页面，局部更新
## ES6相关
### 1.谈一谈let与var和const的区别？
    let为ES6新添加申明变量的命令，它类似于var，但是有以下不同：
    let命令不存在变量提升，如果在let前使用，会导致报错
    暂时性死区的本质，其实还是块级作用域必须“先声明后使用”的性质。
    let，const和class声明的全局变量不是全局对象的属性。
    const声明的变量与let声明的变量类似，它们的不同之处在于，const声明的变量只可以在声明时赋值，不可随意修改，否则会导致SyntaxError（语法错误）。
    const只是保证变量名指向的地址不变，并不保证该地址的数据不变。const可以在多个模块间共享 let 暂时性死区的原因：var 会变量提升，let 不会。
### 2.箭头函数
    箭头函数不属于普通的 function，所以没有独立的上下文。箭头函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象。 由于箭头函数没有自己的this，函数对象中的call、apply、bind三个方法，无法"覆盖"箭头函数中的this值。 箭头函数没有原本(传统)的函数有的隐藏arguments对象。 箭头函数不能当作generators使用，使用yield会产生错误。
    在以下场景中不要使用箭头函数去定义：
      1. 定义对象方法、定义原型方法、定义构造函数、定义事件回调函数。
      2. 箭头函数里不但没有 this，也没有 arguments, super ……
### 3. Map和Set
```javascript
Symbol，Map和Set

Map 对象保存键值对。一个对象的键只能是字符串或者 Symbols，但一个 Map 的键可以是任意值。 Set 对象允许你存储任何类型的唯一值，Set对象是值的集合，Set中的元素只会出现一次 Symbol 是一种特殊的、不可变的数据类型，可以作为对象属性的标识符使用(Symbol([description]) )

let mySet = new Set()
mySet.add(1)
mySet.add('hello')
mySet.add('hello')
console.log(mySet.size);//2
console.log(mySet);//Set {1,'hello'}

//Map保存键值对也不能有重复的
let myMap = new Map();
let key1 = 'China',key2 = 'America';
myMap.set(key1,'welcome')
myMap.set(key2,'gold bless you')
console.log(myMap);//Map { 'China' => 'welcome', 'America' => 'gold bless you' }
console.log(myMap.get(key1));//welcome
console.log(myMap.get(key2));//gold bless you

let mySymbol = Symbol('symbol1');
let mySymbol2 = Symbol('symbol1');
console.log(mySymbol == mySymbol2);//false
//Symbols 在 for...in 迭代中不可枚举。
let obj = {}
obj['c'] = 'c'
obj.d ='d'
obj[Symbol('a')] = 'a'
obj[Symbol.for('b')] = 'b'
for(let k in obj){
    console.log(k);//logs 'c' and 'd'
}
for...of可以用来遍历数组，类数组对象，argument，字符串，Map和Set，for...in用来遍历对象
```
### 4.Promise实现原理
    现在回顾下Promise的实现过程，其主要使用了设计模式中的观察者模式：
    通过Promise.prototype.then和Promise.prototype.catch方法将观察者方法注册到被观察者Promise对象中，同时返回一个新的Promise对象，以便可以链式调用。
    被观察者管理内部pending、fulfilled和rejected的状态转变，同时通过构造函数中传递的resolve和reject方法以主动触发状态转变和通知观察者。
    Promise.then()是异步调用的，这也是Promise设计上规定的，其原因在于同步调用和异步调用同时存在会导致混乱。
    为了暂停当前的 promise，或者要它等待另一个 promise 完成，只需要简单地在 then() 函数中返回另一个 promise。
    Promise 也有一些缺点。首先，无法取消 Promise，一旦新建它就会立即执行，无法中途取消。其次，如果不设置回调函数，Promise 内部抛出的错误，不会反应到外部。第三，当处于 Pending 状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）。
    一般来说，不要在then方法里面定义Reject状态的回调函数（即then的第二个参数），总是使用catch方法，理由是更接近同步的写法。 then的第二个函数参数和catch等价
    Promise.all和Promise.race的区别？
    Promise.all 把多个promise实例当成一个promise实例,当这些实例的状态都发生改变时才会返回一个新的promise实例，才会执行then方法。 Promise.race 只要该数组中的 Promise 对象的状态发生变化（无论是resolve还是reject）该方法都会返回。
### 5.Object.is() 与原来的比较操作符“ ===”、“ ==”的区别？
    两等号判等，会在比较时进行类型转换；
    三等号判等(判断严格)，比较时不进行隐式类型转换,（类型不同则会返回false）；
    Object.is 在三等号判等的基础上特别处理了 NaN 、-0 和 +0 ，保证 -0 和 +0 不再相同，
    但 Object.is(NaN, NaN) 会返回 true.
    Object.is 应被认为有其特殊的用途，而不能用它认为它比其它的相等对比更宽松或严格。
## Ajax
### 1.Ajax
    1.ajax请求和原理
    var xhr = new XMLHTTPRequest();
    // 请求 method 和 URI
    xhr.open('GET', url);
    // 请求内容
    xhr.send();
    // 响应状态
    xhr.status
    // xhr 对象的事件响应
    xhr.onreadystatechange = function() {}
    xhr.readyState
    // 响应内容
    xhr.responseText
    
    AJAX的工作原理
    Ajax的工作原理相当于在用户和服务器之间加了—个中间层(AJAX引擎),使用户操作与服务器响应异步化。　Ajax的原理简单来说通过XmlHttpRequest对象来向服务器发异步请求，从服务器获得数据，然后用javascript来操作DOM而更新页面。
    
    ajax优缺点
    优点：无刷新更新数据 异步与服务器通信 前后端负载均衡
    
    缺点：
    
    1）ajax干掉了Back和history功能，对浏览器机制的破坏 2）对搜索引擎支持较弱 3）违背了URI和资源定位的初衷
    2.fetch和Ajax有什么不同
    XMLHttpRequest 是一个设计粗糙的 API，不符合关注分离（Separation of Concerns）的原则，配置和调用方式非常混乱，而且基于事件的异步模型写起来也没有现代的 Promise，generator/yield，async/await 友好。
    
    fetch 是浏览器提供的一个新的 web API，它用来代替 Ajax（XMLHttpRequest），其提供了更优雅的接口，更灵活强大的功能。 Fetch 优点主要有：
    
        语法简洁，更加语义化
        基于标准 Promise 实现，支持 async/await
    
    fetch(url).then(response => response.json())
      .then(data => console.log(data))
      .catch(e => console.log("Oops, error", e))
### 2. 跨域
    1.JSONP
    回调函数+数据就是 JSON With Padding，简单、易部署。（做法：动态插入script标签，设置其src属性指向提供JSONP服务的URL地址，查询字符串中加入 callback 指定回调函数，返回的 JSON 被包裹在回调函数中以字符串的形式被返回，需将script标签插入body底部）。缺点是只支持GET，不支持POST（原因是通过地址栏传参所以只能使用GET）
    2.document.domain 跨子域
    document.domain 跨子域 （ 例如a.qq.com嵌套一个b.qq.com的iframe ，如果a.qq.com设置document.domain为qq.com 。b.qq.com设置document.domain为qq.com， 那么他俩就能互相通信了，不受跨域限制了。 注意：只能跨子域）
    3. iframe
    window.name + iframe ==> http://www.tuicool.com/articles/viMFbqV，支持跨主域。不支持POST
    4.postMessage()
    HTML5的postMessage()方法允许来自不同源的脚本采用异步方式进行有限的通信，可以实现跨文本档、多窗口、跨域消息传递。适用于不同窗口iframe之间的跨域
    5.CORS
    CORS（Cross Origin Resource Share）对方服务端设置响应头
    设置相应头：”Access-Control-Allow-Origin“
    
    CORS请求默认不发送Cookie和HTTP认证信息。如果要把Cookie发到服务器，一方面要服务器同意，指定Access-Control-Allow-Credentials字段。
    6.服务端代理
    服务端代理 在浏览器客户端不能跨域访问，而服务器端调用HTTP接口只是使用HTTP协议，不会执行JS脚本，不需要同源策略，也就没有跨越问题。简单地说，就是浏览器不能跨域，后台服务器可以跨域。（一种是通过http-proxy-middleware插件设置后端代理；另一种是通过使用http模块发出请求）

### 3.GET和POST的区别
    GET和POST的区别
    
    GET使用URL或Cookie传参，而POST将数据放在BODY中，这个是因为HTTP协议用法的约定。并非它们的本身区别。
    GET方式提交的数据有长度限制，则POST的数据则可以非常大，这个是因为它们使用的操作系统和浏览器设置的不同引起的区别。也不是GET和POST本身的区别。
    POST比GET安全，因为数据在地址栏上不可见，这个说法没毛病，但依然不是GET和POST本身的区别。
    GET和POST最大的区别主要是GET请求是幂等性的，POST请求不是。（幂等性：对同一URL的多个请求应该返回同样的结果。）因为get请求是幂等的，在网络不好的隧道中会尝试重试。如果用get请求增数据，会有重复操作的风险，而这种重复操作可能会导致副作用
### 4.GET,POST,PUT,Delete
    1.  GET请求会向数据库获取信息，只是用来查询数据，不会修改，增加数据。使用URL传递参数，对所发送的数量有限制，一般在2000字符
    2.  POST向服务器发送数据，会改变数据的种类等资源，就像insert操作一样，会创建新的内容，大小一般没有限制，POST安全性高，POST不会被缓存
    3.  PUT请求就像数据库的update操作一样，用来修改数据内容，不会增加数据种类
    4.  Delete用来删除操作
### 5.缓存，存储相关（cookie，web storage和session）
    cookie优点： 1.可以解决HTTP无状态的问题，与服务器进行交互 缺点： 1.数量和长度限制，每个域名最多20条，每个cookie长度不能超过4kb 2.安全性问题。容易被人拦截 3.浪费带宽，每次请求新页面，cookie都会被发送过去
    cookie和session区别
    1.cookie数据存放在客户的浏览器上，session数据放在服务器上。 2.session会在一定时间内保存在服务器上。当访问增多，会比较占用你服务器的性能。考虑到减轻服务器性能方面，应当使用COOKIE。
    sessionStorage是当前对话的缓存，浏览器窗口关闭即消失，localStorage持久存在，除非清除浏览器缓存。
    页面缓存原理
    页面缓存状态是由http header决定的，一个浏览器请求信息，一个是服务器响应信息。主要包括Pragma: no-cache、Cache-Control、 Expires、 Last-Modified、If-Modified-Since。
### 6.什么是Cookie 隔离？（或者说：请求资源的时候不要让它带cookie怎么做）
    如果静态文件都放在主域名下，那静态文件请求的时候都带有的cookie的数据提交给server的，非常浪费流量，
    所以不如隔离开。
      
    因为cookie有域的限制，因此不能跨域提交请求，故使用非主要域名的时候，请求头中就不会带有cookie数据，
    这样可以降低请求头的大小，降低请求时间，从而达到降低整体请求延时的目的。
      
    同时这种方式不会将cookie传入Web Server，也减少了Web Server对cookie的处理分析环节，
    提高了webserver的http请求的解析速度。
### 7. Ajax 解决浏览器缓存问题？
    1、在ajax发送请求前加上 anyAjaxObj.setRequestHeader("If-Modified-Since","0")。
    2、在ajax发送请求前加上 anyAjaxObj.setRequestHeader("Cache-Control","no-cache")。
    3、在URL后面加上一个随机数： "fresh=" + Math.random();。
    4、在URL后面加上时间戳："nowtime=" + new Date().getTime();。
    5、如果是使用jQuery，直接这样就可以了 $.ajaxSetup({cache:false})。这样页面的所有ajax都会执行这条语句就是不需要保存缓存记录。
## Node
### 1.node核心
    核心模块：EventEmitter, Stream, FS, Net和全局对象
    全局对象：process, console, Buffer和exports
    exports和module.exports区别
    exports 是 module.exports 的一个引用 module.exports 初始值为一个空对象 {}，所以 exports 初始值也是 {} require 引用模块后，返回的是 module.exports 而不是 exports
### 2.单线程优点
    Node.js依托于v8引擎，都是以单线程为基础的。单线程资源占用小。单线程避免了传统PHP那样频繁创建、切换线程的开销，使执行速度更加迅速
### 3.Node.js是如何做到I/O的异步和非阻塞的呢
    其实Node在底层访问I/O还是多线程的。Node可以借助livuv来来实现多线程。
    如果我们非要让Node.js支持多线程，还是提倡使用官方的做法，利用libuv库来实现。
    cluster可以用来让Node.js充分利用多核cpu的性能
### 4.并行与并发，进程与线程
    并发 (Concurrent) = 2 队列对应 1 咖啡机.
    并行 (Parallel) = 2 队列对应 2 咖啡机.
    线程是进程下的执行者，一个进程至少会开启一个线程（主线程），也可以开启多个线程。
### 5.谈谈Nodejs优缺点
    优点：
    1. 事件驱动，异步编程，占用内存少
    2. npm设计得好
    缺点：
    1. Debug 很困难。没有 stack trace，出了问题很难查找问题的原因；
    2 .如果设计不好，很容易让代码充满 callback，代码不优雅；
    3. 可靠性低；
    4. 单进程，单线程，只支持单核CPU，不能充分的利用多核CPU服务器。
## 模块化
### 1.模块化开发怎么做？
```javascript
立即执行函数,不暴露私有成员
 var module1 = (function(){
　　　　var _count = 0;
　　　　var m1 = function(){
　　　　　　//...
　　　　};
　　　　var m2 = function(){
　　　　　　//...
　　　　};
　　　　return {
　　　　　　m1 : m1,
　　　　　　m2 : m2
　　　　};
　　})();
（待完善）
```
### 2.AMD、CMD规范区别？
```javascript
AMD(Modules/Asynchronous-Definition)规范在这里：https://github.com/amdjs/amdjs-api/wiki/AMD
CMD(Common Module Definition)规范在这里：https://github.com/seajs/seajs/issues/242
Asynchronous Module Definition，异步模块定义，所有的模块将被异步加载，模块加载不影响后面语句运行。所有依赖某些模块的语句均放置在回调函数中。
  区别：
     1. 对于依赖的模块，AMD 是提前执行，CMD 是延迟执行。不过 RequireJS 从 2.0 开始，也改成可以延迟执行（根据写法不同，处理方式不同）。CMD 推崇 as lazy as possible.
     2. CMD 推崇依赖就近，AMD 推崇依赖前置。看代码：
 // CMD
 define(function(require, exports, module) {
     var a = require('./a')
     a.doSomething()
     // 此处略去 100 行
     var b = require('./b') // 依赖可以就近书写
     b.doSomething()
     // ...
 })

 // AMD 默认推荐
 define(['./a', './b'], function(a, b) { // 依赖必须一开始就写好
     a.doSomething()
     // 此处略去 100 行
     b.doSomething()
     // ...
 })
```
### 3.requireJS的核心原理是什么？（如何动态加载的？如何避免多次加载的？如何 缓存的？）
    参考：http://annn.me/how-to-realize-cmd-loader/
## 构建工具
### 1.webpack相关问题
    1.loader和plugin区别
    loader用于加载某些资源文件，因为webpack本身只能打包CommonJS规范的js文件，对于其他资源，例如css，图片等，是没有办法加载的，这就需要对应的loader将资源转换 plugin用于扩展webpack的功能，直接作用于webpack，loader只专注于转换文件，而plugin不仅局限于资源加载
    
    Loader只能处理单一文件的输入输出，而Plugin则可以对整个打包过程获得更多的灵活性，譬如 ExtractTextPlugin，它可以将所有文件中的css剥离到一个独立的文件中，这样样式就不会随着组件加载而加载了。
### 2.什么是chunk
    Webpack提供一个功能可以拆分模块，每一个模块称为chunk，这个功能叫做Code Splitting。你可以在你的代码库中定义分割点，调用require.ensure，实现按需加载 

### 3.如何开发一个loader，原理是啥
    A loader is a node module exporting a function.
    缓存： Webpack Loader 同样可以利用缓存来提高效率，并且只需在一个可缓存的 Loader 上加一句 this.cacheable() 异步：在一个异步的模块中，回传时需要调用 Loader API 提供的回调方法 this.async()
### 4.打包原理
```javascript
webpack打包，最基本的实现方式，是将所有的模块代码放到一个数组里，通过数组ID来引用不同的模块

/************************************************************************/
/******/ ([
/* 0 */
/***/ function(module, exports, __webpack_require__) {

    __webpack_require__(1);
    __webpack_require__(2);
    console.log('Hello, world!');

/***/ },
/* 1 */
/***/ function(module, exports) {

    var a = 'a.js';
    console.log("I'm a.js");

/***/ },
/* 2 */
/***/ function(module, exports) {

    var b = 'b.js';
    console.log("I'm b.js");

/***/ }
/******/ ]);
可以发现入口entry.js的代码是放在数组索引0的位置，其它a.js和b.js的代码分别放在了数组索引1和2的位置，而webpack引用的时候，主要通过__webpack_require__的方法引用不同索引的模块。
```
### 5.webpack和gulp的区别
    webpack是一种模块化打包工具，主要用于模块化方案，预编译模块的方案；gulp是工具链、构建工具，可以配合各种插件做js压缩，css压缩，less编译 替代手工实现自动化工作。
    Grunt/Gulp更多的是一种工作流；提供集成所有服务的一站式平台； gulp可以用来优化前端工作流程。
### 6.如何写一个plugin
    Compiler在开始打包时就进行实例化，实例对象里面装着与打包相关的环境和参数，包括options、plugins和loaders等。
    compilation对象，它继承于compiler，所以能拿到一切compiler的内容。Compilation表示有关模块资源，已编译资源，Compilation在每次文件变化重新打包时都进行一次实例化
    apply方法：当安装这个插件的时候，这个apply方法就会被webpack compiler调用。
    function HelloWorldPlugin(options) {
      // Setup the plugin instance with options...
    }
    HelloWorldPlugin.prototype.apply = function(compiler) {
      compiler.plugin('done', function() {
        console.log('Hello World!');
      });
    };
    module.exports = HelloWorldPlugin;
### 7.webpack打包后文件体积过大怎么办？
    很多方法：异步加载模块（代码分割）；提取第三方库（使用cdn或者vender）；代码压缩；去除不必要的插件；去除devtool选项，dllplugin等等。
## jQuery
### 1.jQuery.fn的init方法返回的this指的是什么对象？为什么要返回this？
    this执行init构造函数自身，其实就是jQuery实例对象，返回this是为了实现jQuery的链式操作
### 2.jquery中如何将数组转化为json字符串，然后再转化回来？
    jQuery中没有提供这个功能，所以你需要先编写两个jQuery的扩展：
    $.fn.stringifyArray = function(array) {
        return JSON.stringify(array)
    }
    $.fn.parseArray = function(array) {
        return JSON.parse(array)
    }
    然后调用：
    $("").stringifyArray(array)
### 3.jQuery 的属性拷贝(extend)的实现原理是什么，如何实现深拷贝？
    递归赋值
### 4.jquery.extend 与 jquery.fn.extend的区别？
    * jquery.extend 为jquery类添加类方法，可以理解为添加静态方法
    * jquery.fn.extend:
      源码中jquery.fn = jquery.prototype，所以对jquery.fn的扩展，就是为jquery类添加成员函数
    使用：
    jquery.extend扩展，需要通过jquery类来调用，而jquery.fn.extend扩展，所有jquery实例都可以直接调用。
### 5.针对 jQuery 的优化方法？
    *基于Class的选择性的性能相对于Id选择器开销很大，因为需遍历所有DOM元素。
    
    *频繁操作的DOM，先缓存起来再操作。用Jquery的链式调用更好。
    比如：var str=$("a").attr("href");
    
    *for (var i = size; i < arr.length; i++) {}
    for 循环每一次循环都查找了数组 (arr) 的.length 属性，在开始循环的时候设置一个变量来存储这个数字，可以让循环跑得更快：
    for (var i = size, length = arr.length; i < length; i++) {}
### 6.JQuery一个对象可以同时绑定多个事件，这是如何实现的？
    * 多个事件同一个函数：
      $("div").on("click mouseover", function(){});
    * 多个事件不同函数
      $("div").on({
           click: function(){},
           mouseover: function(){}
      });
### 7.bind(), live(), delegate()的区别
    bind： 绑定事件，对新添加的事件不起作用，方法用于将一个处理程序附加到每个匹配元素的事件上并返回jQuery对象。
    live： 方法将一个事件处理程序附加到与当前选择器匹配的所有元素（包含现有的或将来添加的）的指定事件上并返回jQuery对象。
    delegate： 方法基于一组特定的根元素将处理程序附加到匹配选择器的所有元素（现有的或将来的）的一个或多个事件上。
### 8.jQuery框架中$.ajax()的常用参数有哪些？写一个post请求并带有发送数据和返回数据的样例
```javascript
async是否异步
url请求地址
contentType发送信息至服务器时内容编码类型
data发送到服务器的数据
dataType预期服务器返回的数据类型
type请求类型
success请求成功回调函数
error请求失败回调函数
$.ajax({
    url: "/jquery/test1.txt",
    type: 'post',
    data: {
        id: 1
    },
    success: function(data) {
        alert(data);
    }
}
```
### 9.jQuery一个对象可以同时绑定多个事件，这是如何实现的？
    jQuery可以给一个对象同时绑定多个事件，低层实现方式是使用addEventListner或attachEvent兼容不同的浏览器实现事件的绑定，这样可以给同一个对象注册多个事件。
### 10.jquery 中如何将数组转化为json字符串，然后再转化回来？
    jQuery中没有提供这个功能，所以你需要先编写两个jQuery的扩展：
    $.fn.stringifyArray = function(array) {
        return JSON.stringify(array)
    }
    $.fn.parseArray = function(array) {
        return JSON.parse(array)
    } 
    然后调用：
    $("").stringifyArray(array)
### 11.Jquery与jQuery UI有啥区别？
    jQuery是操作dom的框架，jQueryUI是基于jQuery做的一个UI组件库
### 12.jQuery和Zepto的区别？各自的使用场景？
    jQuery主要用于pc端，当然有对应的jQuerymobile用于移动端，zepto比jQuery更加小巧，主要用于移动端
### 13.针对 jQuery 的优化方法？
    优先使用ID选择器
    在class前使用tag(标签名)
    给选择器一个上下文
    慎用 .live()方法（应该说尽量不要使用）
    使用data()方法存储临时变量
### 14.Zepto的点透问题如何解决？
    点透主要是由于两个div重合，例如：一个div调用show()，一个div调用hide()；这个时候当点击上面的div的时候就会影响到下面的那个div；
    解决办法主要有2种：
    1.github上有一个叫做fastclick的库，它也能规避移动设备上click事件的延迟响应，https://github.com/ftlabs/fastclick
    将它用script标签引入页面（该库支持AMD，于是你也可以按照AMD规范，用诸如require.js的模块加载器引入），并且在dom ready时初始化在body上，
    2.根据分析，如果不引入其它类库，也不想自己按照上述fastclcik的思路再开发一套东西，需要1.一个优先于下面的“divClickUnder”捕获的事件；2.并且通过这个事件阻止掉默认行为（下面的“divClickUnder”对click事件的捕获，在ios的safari，click的捕获被认为和滚屏、点击输入框弹起键盘等一样，是一种浏览器默认行为，即可以被event.preventDefault()阻止的行为）。
## React
### 1.react和vue的区别
    相同点：
      都支持服务端渲染
      都有Virtual DOM，组件化开发，通过props参数进行父子组件数据的传递，都实现webComponents规范
      数据驱动视图
      都有支持native的方案，React的React native，Vue的weex
    不同点：
      React严格上只针对MVC的view层，Vue则是MVVM模式
      virtual DOM 不一样 vue会跟踪每一个组件的依赖关系，不需要重新渲染整个组件树。而对于React而言，每当应用的状态被改变时，全部子组件都会重新渲染。当然，这可以通过shouldComponentUpdate这个生命周期方法来进行控制，
      组件写法不一样 React 推荐的做法是 JSX + inline style，也就是把 HTML 和 CSS 全都写进 JavaScript 了，即”all in js” Vue 推荐的是使用 webpack + vue-loader 的单文件组件格式，即html,css,js写在同一个文件；
      数据绑定：Vue有实现了双向数据绑定，React数据流动是单向的
      state对象在react应用中是不可变的，需要使用setState方法更新状态；在Vue中，state对象并不是必须的，数据由data属性在Vue对象中进行管理。
### 2.react的优缺点
    优点：
    1. 可以通过函数式方法描述视图组件（好处：相同的输入会得到同样的渲染结果，不会有副 作  用；组件不会被实例化，整体渲染性能得到提升）
    2. 集成虚拟DOM（性能好）
    3. 单向数据流（好处是更容易追踪数据变化排查问题
    4. 一切都是component：代码更加模块化，重用代码更容易，可维护性高
    5. 大量拥抱 es6 新特性
    6: jsx
    缺点：
    1. jsx的一个问题是，渲染函数常常包含大量逻辑，最终看着更像是程序片段，而不是视觉呈现。后期如果发生需求更改，维护起来工作量将是巨大的
    2. 大而全，上手有难度
### 3.jsx的优缺点
    允许使用熟悉的语法来定义HTML元素树 JSX 让小组件更加简单、明了、直观。 更加语义化且易懂的标签 JSX 本质是对JavaScript语法的一个扩展，看起来像是某种模板语言，但其实不是。但正因为形似HTML，描述UI就更直观了，也极大地方便了开发； 在React中babel会将JSX转换为React.createElement函数调用，然后将JSX转换为正确的JSON对象（VDOM 也是一个“树”形的结构） React/JSX乍看之下，觉得非常啰嗦，但使用JavaScript而不是模板语法来开发（模板语法比较有局限性），赋予了开发者许多编程能力。
### 4.dom diff算法和虚拟DOM
    React中的render方法，返回一个DOM描述，结果仅仅是轻量级的js对象。Reactjs只在调用setState的时候会更新dom，而且还是先更新Virtual Dom，然后和实际DOM比较，最后再更新实际DOM。
    React.js 厉害的地方并不是说它比 DOM 快（这句话本来就是错的），而是说不管你数据怎么变化，我都可以以最小的代价来更新 DOM。方法就是我在内存里面用新的数据刷新一个虚拟的 DOM 树，然后新旧 DOM 树进行比较，找出差异，再更新到真正的 DOM 树上。
    当我们修改了DOM树上一些节点对应绑定的state，React会立即将它标记为“脏状态”。在事件循环的最后才重新渲染所有的脏节点。在实际的代码中，会对新旧两棵树进行一个深度优先的遍历，这样每个节点都会有一个唯一的标记，每遍历到一个节点就把该节点和新的的树进行对比。如果有差异的话就记录到一个对象里面，最后把差异应用到真正的DOM树上。 

#### 算法实现 
    步骤一：用JS对象模拟DOM树 
    步骤二：比较两棵虚拟DOM树的差异 
    步骤三：把差异应用到真正的DOM树上 这就是所谓的 diff 算法
    dom diff采用的是增量更新的方式，类似于打补丁。React 需要为数据添加 key 来保证虚拟 DOM diff 算法的效率。key属性可以帮助React定位到正确的节点进行比较，从而大幅减少DOM操作次数，提高了性能。
    virtual dom，也就是虚拟节点。它通过JS的Object对象模拟DOM中的节点，然后再通过特定的render方法将其渲染成真实的DOM节点。 http://react-china.org/t/dom/638

#### 为什么js对象模拟DOM会比js操作DOM来得快:
    为了解决频繁操作DOM导致Web应用效率下降的问题，React提出了“虚拟DOM”（virtual DOM）的概念。Virtual DOM是使用JavaScript对象模拟DOM的一种对象结构。DOM树中所有的信息都可以用JavaScript表述出来，例如：
    <ul>
      <li>Item 1</li>
      <li>Item 2</li>
      <li>Item 3</li>
    </ul>
    可以用以下JavaScript对象来表示：
    {
      tag: 'ul',
      children: [{
        tag: 'li', children: ['Item 1'],
        tag: 'li', children: ['Item 2'],
        tag: 'li', children: ['Item 3']
      }]
    }
    这样可以避免直接频繁地操作DOM，只需要在js对象模拟的虚拟DOM进行比对，再将更改的部分应用到真实的DOM树上
#### react组件性能优化
    使用PureRenderMixin、shouldComponentUpdate来避免不必要的虚拟DOM diff，在render内部优化虚拟DOM的diff速度，以及让diff结果最小化
### 5.react组件间的数据传递
    1.兄弟组件不能直接相互传送数据，此时可以将数据挂载在父组件中，由两个组件共享
    2.子组件向父组件通讯，可以通过父组件定义事件（回调函数），子组件调用该函数，通过实参的形式来改变父组件的数据来通信
    //子组件
    this.props.onCommentSubmit({author, content, date:new Date().getTime()});
    //父组件
    render(){
        return(
          <div className="m-index">
            <div>
              <h1>评论</h1>
            </div>
            <CommentList data={this.state.data} />
            <CommentForm onCommentSubmit={this.handleCommentSubmit.bind(this)} />
          </div>
        )
    }
    3.非父子组件间的通信：可以使用全局事件来实现组件间的沟通，React中可以引入eventProxy模块，利用eventProxy.trigger()方法发布消息，eventProxy.on()方法监听并接收消息。
    
    4.组件间层级太深，可以使用上下文方式，让子组件直接访问祖先的数据或函数，通过this.context.xx
### 6.无状态组件
    无状态组件其实本质上就是一个函数，传入props即可，没有state，也没有生命周期方法。组件本身对应的就是render方法。例子如下：
    function Title({color = 'red', text = '标题'}) {
      let style = {
        'color': color
      }
      return (
        <div style = {style}>{text}</div>
      )
    }
    无状态组件不会创建对象，故比较省内存。没有复杂的生命周期方法调用，故流程比较简单。没有state，也不会重复渲染。它本质上就是一个函数而已。
    对于没有状态变化的组件，React建议我们使用无状态组件。总之，能用无状态组件的地方，就用无状态组件。
### 7.高阶组件
    高阶组件（HOC）是函数接受一个组件，返回一个新组件。其前身其实是用ES5创建组件时可用的mixin方法，但是在react版本升级过程中，使用ES6语法创建组件时，认为mixin是反模式，影响了react架构组件的封装稳定性，增加了不可控的复杂度，逐渐被HOC所替代。 实现高阶组件的方式有：
    
    属性代理
    import React, { Component } from 'React';
    //高阶组件定义
    const HOC = (WrappedComponent) =>
      class WrapperComponent extends Component {
        render() {
          return <WrappedComponent {...this.props} />;
        }
    }
    //普通的组件
    class WrappedComponent extends Component{
        render(){
            //....
        }
    }
    //高阶组件使用
    export default HOC(WrappedComponent)
    反向继承
    反向继承是指返回的组件去继承之前的组件(这里都用WrappedComponent代指)
    const HOC = (WrappedComponent) =>
      class extends WrappedComponent {
        render() {
          return super.render();
        }
      }
    我们可以看见返回的组件确实都继承自WrappedComponent,那么所有的调用将是反向调用的(例如:super.render())，这也就是为什么叫做反向继承。 　　
### 8.react事件和传统事件有什么区别吗
    React 实现了一个“合成事件”层（synthetic event system），这个事件模型保证了和 W3C 标准保持一致，所以不用担心有什么诡异的用法，并且这个事件层消除了 IE 与 W3C 标准实现之间的兼容问题。
    
    “合成事件”还提供了额外的好处：
    
    事件委托
    “合成事件”会以事件委托（event delegation）的方式绑定到组件最上层，并且在组件卸载（unmount）的时候自动销毁绑定的事件。
### 9.react的生命周期图
![React生命周期](https://upload-images.jianshu.io/upload_images/4118241-d979d05af0b7d4db.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/488/format/webp)
```
react组件更新过程：
props/state change：
1.componentWillReceiveProps(nextProps)
只要是父组件的render被调用，在render中被渲染的子组件就会经历更新的过程。不管父组件传给子组件的props有没有改变，都会触发子组件的此函数被调用。注意：通过setState方法触发的更新不会调用此函数
2.shouldComponentUpdate(nextProps,nextState) 
3.componentWillUpdate 
4.render 
5.componentDidUpdate
```
## Vue
### 1.vue 双向绑定底层实现原理
    vue.js 采用数据劫持的方式，结合发布者-订阅者模式，通过Object.defineProperty()来劫持各个属性的setter，getter以监听属性的变动，在数据变动时发布消息给订阅者，触发相应的监听回调：
    https://github.com/hawx1993/tech-blog/issues/11
### 2.vue 虚拟DOM和react 虚拟DOM的区别
    在渲染过程中，会跟踪每一个组件的依赖关系，不需要重新渲染整个组件树。而对于React而言，每当应用的状态被改变时，全部子组件都会重新渲染。 在 React 应用中，当某个组件的状态发生变化时，它会以该组件为根，重新渲染整个组件子树。 如要避免不必要的子组件的重新渲染，你需要在所有可能的地方使用 PureComponent，或是手动实现shouldComponentUpdate 方法
    
    在React中，数据流是自上而下单向的从父节点传递到子节点，所以组件是简单且容易把握的，子组件只需要从父节点提供的props中获取数据并渲染即可。如果顶层组件的某个prop改变了，React会递归地向下遍历整棵组件树，重新渲染所有使用这个属性的组件。
### 3.v-show和v-if区别
    与v-if不同的是，无论v-show的值为true或false，元素都会存在于HTML代码中；而只有当v-if的值为true，元素才会存在于HTML代码中
### 4.vue组件通信
    非父子组件间通信，Vue 有提供 Vuex，以状态共享方式来实现同信，对于这一点，应该注意考虑平衡，从整体设计角度去考量，确保引入她的必要。
    父传子: this.$refs.xxx 子传父: this.$parent.xxx
    还可以通过$emit方法出发一个消息，然后$on接收这个消息
### 5.你如何评价vue
    框架能够让我们跑的更快，但只有了解原生的JS才能让我们走的更远。
    vue专注于MVVM中的viewModel层，通过双向数据绑定，把view层和Model层连接了起来。核心是用数据来驱动DOM。这种把directive和component混在一起的设计有一个非常大的问题，它导致了很多开发者滥用Directive（指令），出现了到处都是指令的情况。
    优点： 
    1.不需要setState，直接修改数据就能刷新页面，而且不需要react的shouldComponentUpdate就能实现最高效的渲染路径。
    2.渐进式的开发模式，模版方式->组件方式->路由整合->数据流整合->服务器渲染。
    3.上手的曲线更加平滑简单，而且不像react一上来就是组件全家桶 
    4.v-model给开发后台管理系统带来极大的便利，反观用react开发后台就是个杯具 4.html，css与js比react更优雅地结合在一个文件上。
    缺点：
    1.指令太多，自带模板扩展不方便； 
    2.组件的属性传递没有react的直观和明显
### 6.说说你对MVVM的理解
    Model层代表数据模型，可以在Model中定义数据修改和操作业务逻辑； view 代表UI组件。负责将数据模型转换成UI展现出来 ViewModel 是一个同步View和Model的对象
    用户操作view层，view数据变化会同步到Model，Model数据变化会立即反应到view中。viewModel通过双向数据绑定把view层和Model层连接了起来
### 7.为什么选择vue
    reactjs 的全家桶方式，实在太过强势，而自己定义的 JSX 规范，揉和在 JS 的组件框架里，导致如果后期发生页面改版工作，工作量将会巨大。
    vue的核心：数据绑定 和 视图组件。
    Vue的数据驱动：数据改变驱动了视图的自动更新，传统的做法你得手动改变DOM来改变视图，vuejs只需要改变数据，就会自动改变视图，一个字：爽。再也不用你去操心DOM的更新了，这就是MVVM思想的实现。
    视图组件化：把整一个网页的拆分成一个个区块，每个区块我们可以看作成一个组件。网页由多个组件拼接或者嵌套组成
### 8.vue中mixin与extend区别
    全局注册混合对象，会影响到所有之后创建的vue实例，而Vue.extend是对单个实例进行扩展。
    mixin 混合对象（组件复用）
    同名钩子函数（bind，inserted，update，componentUpdate，unbind）将混合为一个数组，因此都将被调用，混合对象的钩子将在组件自身钩子之前调用
    methods，components，directives将被混为同一个对象。两个对象的键名（方法名，属性名）冲突时，取组件（而非mixin）对象的键值对
### 9.双向绑定和单向数据绑定的优缺点
    只有 UI控件 才存在双向，非 UI控件 只有单向。 
    单向绑定的优点:
    1.可以带来单向数据流，这样的好处是流动方向可以跟踪，流动单一，没有状态, 这使得单向绑定能够避免状态管理在复杂度上升时产生的各种问题, 程序的调试会变得相对容易。
    2.单向数据流更利于状态的维护及优化，更利于组件之间的通信，更利于组件的复用
    
    双向数据流的优点：
    无需进行和单向数据绑定的那些CRUD（Create，Retrieve，Update，Delete）操作； 
    在一些需要实时反应用户输入的场合会非常方便，用户在视图上的修改会自动同步到数据模型中去，数据模型中值的变化也会立刻同步到视图中去；
    缺点：
    双向数据流是自动管理状态的, 但是在实际应用中会有很多不得不手动处理状态变化的逻辑, 使得程序复杂度上升 无法追踪局部状态的变化 双向数据流，值和UI绑定，但由于各种数据相互依赖相互绑定，导致数据问题的源头难以被跟踪到
    
    Vue 虽然通过 v-model 支持双向绑定，但是如果引入了类似redux的vuex，就无法同时使用 v-model。
    双绑跟单向绑定之间的差异只在于，双向绑定把数据变更的操作隐藏在框架内部，调用者并不会直接感知。
    <input v-model="something">
    <!-- 等价于以下内容 -->
    <input :value="something" @input="something = $event.target.value">
    也就是说，你只需要在组件中声明一个name为value的props，并且通过触发input事件传入一个值，就能修改这个value。
### 10.VueJS 开发常见问题集锦
    https://segmentfault.com/a/1190000010230843
### 11.谈谈你对组件的理解
    一个组件应该有以下特征：
    1.  可组合（Composeable）：一个组件易于和其它组件一起使用，或者嵌套在另一个组件内部。如果一个组件内部创建了另一个组件，那么说父组件拥有（own）它创建的子组件，通过这个特性，一个复杂的 2.  UI 可以拆分成多个简单的 UI 组件；
    3.  可重用（Reusable）：每个组件都是具有独立功能的，它可以被使用在多个 UI 场景；
    4.  可维护（Maintainable）：每个小的组件仅仅包含自身的逻辑，更容易被理解和维护；
    5.  可测试（Testable）：因为每个组件都是独立的，那么对于各个组件分别测试显然要比对于整个 UI 进行测试容易的多。
## 移动端问题
### 1.说说你知道的移动端web的兼容性bug
    1、一些情况下对非可点击元素如(label,span)监听click事件，ios下不会触发，css增加cursor:pointer就搞定了。
    2.position 在Safari下的两个定位需要都写，只写一个容易发生错乱
    3.Input 的placeholder会出现文本位置偏上的情况
    input 的placeholder会出现文本位置偏上的情况：PC端设置line-height等于height能够对齐，而移动端仍然是偏上，解决是设置line-height：normal
    4.zepto点击穿透问题
    引入fastclick解决；event.preventDefault
    5.当输入框在最底部的时候，弹起的虚拟键盘会把输入框挡住。
    Element.scrollIntoViewIfNeeded(opt_center)
### 2.有哪些多屏适配方案
    1.  media query + rem
    2.  flex
    3.  弹性布局
    4.  flexiable 整体缩放（动态设置缩放系数的方式， 让layout viewport与设计图对应，极大地  方便了重构，同时也避免了1px的问题）
### 3.移动端的布局用过媒体查询吗？
    假设你现在正用一台显示设备来阅读这篇文章，同时你也想把它投影到屏幕上，或者打印出来， 而显示设备、屏幕投影和打印等这些媒介都有自己的特点，CSS就是为文档提供在不同媒介上展示的适配方法
    当媒体查询为真时，相关的样式表或样式规则会按照正常的级联规被应用。 当媒体查询返回假， 标签上带有媒体查询的样式表 仍将被下载 （只不过不会被应用）。
    包含了一个媒体类型和至少一个使用 宽度、高度和颜色等媒体属性来限制样式表范围的表达式。 CSS3加入的媒体查询使得无需修改内容便可以使样式应用于某些特定的设备范围。
    <style> @media (min-width: 700px) and (orientation: landscape){ .sidebar { display: none; } } </style>
### 4.移动端（Android IOS）怎么做好用户体验?
    清晰的视觉纵线、
    信息的分组、极致的减法、
    利用选择代替输入、
    标签及文字的排布方式、
    依靠明文确认密码、
    合理的键盘利用、
### 5.移动端最小触控区域是多大？
    移动端的点击事件的有延迟，时间是多久，为什么会有？ 怎么解决这个延时？（click 有 300ms 延迟,为了实现safari的双击事件的设计，浏览器要知道你是不是要双击操作。）
## 美团面试题
### 1.什么是事件循环
    浏览器中, js引擎线程会循环从 任务队列 中读取事件并且执行, 这种运行机制称作 Event Loop (事件循环).
    每个浏览器环境，至多有一个event loop。 一个event loop可以有1个或多个task queue(任务队列)
    先执行同步的代码，然后js会跑去消息队列中执行异步的代码，异步完成后，再轮到回调函数，然后是去下个事件循环中执行setTimeout
    它从script(整体代码)开始第一次循环。之后全局上下文进入函数调用栈。直到调用栈清空(只剩全局)，然后执行所有的micro-task。当所有可执行的micro-task执行完毕之后。循环再次从macro-task开始，找到其中一个任务队列执行完毕，然后再执行所有的micro-task，这样一直循环下去。
    从规范上来讲，setTimeout有一个4ms的最短时间，也就是说不管你设定多少，反正最少都要间隔4ms才运行里面的回调。而Promise的异步没有这个问题。Promise所在的那个异步队列优先级要高一些 Promise是异步的，是指他的then()和catch()方法，Promise本身还是同步的 Promise的任务会在当前事件循环末尾中执行，而setTimeout中的任务是在下一次事件循环执行
    //依次输出 12354
    setTimeout(function(){
      console.log(4)
      },0);
    new Promise(function(resolve){
      console.log(1)
      for( var i=0 ; i<10000 ; i++ ){
        i===9999 && resolve()
      }
      console.log(2)
    }).then(function(){
      console.log(5)
    });
    console.log(3);
    xss和csrf
    事件捕获的应用
    jsx的优点
    webpack loader和plugin区别
    性能优化
    react和vue的区别
    vue component和指令的区别
    vue组件通信
    box-sizing
    jsonp缺点，为什么不能用POST
    vue-router的实现原理
    es6用了哪些新特性
    cookie和localStorage区别
    git fetch是干嘛的
    事件代理和冒泡，捕获
    304是干嘛的 具体，405 504又是干嘛的
    BFC
    其他（自我介绍，为啥离职，为啥从美团离职，git工作流，code review，单元测试）
    react组件生命周期
    伪类和伪元素的区别 CSS 伪类：逻辑上存在但在文档树中却无须标识的“幽灵”分类 CSS 伪元素（:first-letter，:first-line,:after,:before）代表了某个元素的子元素，这个子元素虽然在逻辑上存在，但却并不实际存在于文档树中。 CSS3标准要求伪元素使用双冒号
    em和rem
## 饿了么面试题
### 1.什么是类数组对象，如何将类数组对象转为真正的数组 
```javascript
拥有length属性和若干索引属性的对象, 类数组只有索引值和长度，没有数组的各种方法，所以如果要类数组调用数组的方法，就需要使用 Array.prototype.method.call 来实现。
var arrayLike = {0: 'name', 1: 'age', 2: 'sex', length: 3 }
// 1. slice
Array.prototype.slice.call(arrayLike); // ["name", "age", "sex"]
// 2. splice
Array.prototype.splice.call(arrayLike, 0); // ["name", "age", "sex"]
// 3. ES6 Array.from
Array.from(arrayLike); // ["name", "age", "sex"]
// 4. apply
Array.prototype.concat.apply([], arrayLike)
```
### 2.跨域
### 3.伪元素和伪类
    伪类用于当已有元素处于的某个状态时，为其添加对应的样式，这个状态是根据用户行为而动态变化的。
    a:link
    :first-child
    :nth-child
    :focus
    :visited
    伪元素代表了某个元素的子元素，这个子元素虽然在逻辑上存在，但却并不实际存在于文档树中。
### 4.bind返回什么
```javascript
bind() 方法会返回一个新函数, 又叫绑定函数, 当调用这个绑定函数时, 绑定函数会以创建它时传入 bind() 方法的第一个参数作为当前的上下文, 即this, 传入 bind() 方法的第二个及之后的参数加上绑定函数运行时自身的参数按照顺序作为原函数的参数来调用原函数.
var x = 8;
var o = {
  x: 10,
  getX: function(){
    console.log(this.x);
  }
};
var f = o.getX;
f();//8, 由于没有绑定执行时的上下文, this默认指向window, 打印了全局变量x的值
var g = f.bind(o);
g();//10, 绑定this后, 成功的打印了o对象的x属性的值.
```
### 5.git rebase和git merge的区别
    merge操作会生成一个新的节点，之前的提交分开显示。而rebase操作不会生成新的节点，是将两个分支融合成一个线性的提交。
### 6.箭头函数 箭头函数没有它自己的this值，箭头函数内的this值继承自外围作用域
    箭头函数不能用作构造器，不能和new一起使用 箭头函数没有原型属性 yield关键字不能在箭头函数使用 在以下场景中不要使用箭头函数去定义：
    定义对象方法、定义原型方法、定义构造函数、定义事件回调函数。
### 7.== 和===的区别
    相等运算符在比较相同类型的数据时，与严格相等运算符完全一样。
    在比较不同类型的数据时，相等运算符会先将数据进行类型转换，然后再用严格相等运算符比较。
## 糯米面试题
### 1. js有几种数据类型。
### 2. 将一个字符串转化为数字，将数字转化为字符串。
### 3. 将数字转化为布尔型。
### 4. 对数组的几种操作方式。
### 5. 获取当前网址url并从中截取信息。
### 6. postison的几种属性
### 7. rem移动端
### 8. 事件委托原理
### 9. 闭包原理并应用
### 10. 原型继承的几种方式
### 11. jsonp跨域问题，并手写代码
### 12. 实现斐波那契数列，并手写代码
### 13. 数组去重，并手写代码
## 扩展性问题
### 1.职业规划：
    1.首先应该是一个优秀的程序员
    2.其次是努力使自己成为某一领域的技术专家
    3.通过技术更好的服务于团队和业务
    4.提高沟通能力，团队协作，发现问题，解决问题，总结问题能力
    5.写写博客，输出就是最好的学习
    6.提升个人前端的工作效率和工作质量
    7.关注前端前言技术和发展方向，通过新技术服务团队和业务
    8.一专多长，想成为优秀的前端工程师，首先在专业技能领域必不可少，其次在团队贡献、业务思索、价值判断上也有要求。这三方面能决定你的专业技能能够为公司产出多大的价值。我觉得程序员最核心的竞争力是学习力和责任。 学习能力的源泉就是好奇心，也就是对新知识的渴求，以及对探索未知的冲动。
### 2.你希望加入一个什么样的团队
    1.对前端开发有激情
    2.能够持之以恒的学习
    3.团队做事方式是否规范（代码规范，安全规范，流程规范）
    4.团队有足够的成长空间，对自己有个清晰的定位。
    5.团队认可我的价值
### 3.最后你有什么要问我的吗
    1.可以问一下公司具体的情况，比如我即将加入的部门的主要业务
    2.问一下具体工作情况，比如需要做哪些内容
    3.公司的氛围和公司的文化
### 4.贵司对这项职务的工作内容和期望目标
### 5.原来公司工作流程是怎么样的，如何与其他人协作的？如何跨部门合作的？
### 6.你遇到过比较难的技术问题是？你是如何解决的？
### 7.对前端工程师这个职位是怎么样理解的？它的前景会怎么样？
  前端是最贴近用户的程序员，比后端、数据库、产品经理、运营、安全都近。
  1、实现界面交互
  2、提升用户体验
  3、有了Node.js，前端可以实现服务端的一些事情
  前端是最贴近用户的程序员，前端的能力就是能让产品从 90分进化到 100 分，甚至更好，
  参与项目，快速高质量完成实现效果图，精确到1px；
  与团队成员，UI设计，产品经理的沟通；
  做好的页面结构，页面重构和用户体验；
  处理hack，兼容、写出优美的代码格式；
  针对服务器的优化、拥抱最新前端技术。
### 8.你对加班的看法？
  加班就像借钱，原则应当是------救急不救穷
### 9.你在现在的团队处于什么样的角色，起到了什么明显的作用？
## 其他问题
### 1.常使用的库有哪些？常用的前端开发工具？开发过什么应用或组件？
### 2.页面重构怎么操作？
    网站重构：在不改变外部行为的前提下，简化结构、添加可读性，而在网站前端保持一致的行为，也就是说是在不改变UI的情况下，对网站进行优化，在扩展的同时保持一致的UI。
    对于传统的网站来说重构通常是：
      表格(table)布局改为DIV+CSS
      使网站前端兼容于现代浏览器(针对于不合规范的CSS、如对IE6有效的)
      对于移动平台的优化
      针对于SEO进行优化
    深层次的网站重构应该考虑的方面：
      减少代码间的耦合
      让代码保持弹性
      严格按规范编写代码
      设计可扩展的API
      代替旧有的框架、语言(如VB)
      增强用户体验
      通常来说对于速度的优化也包含在重构中
      压缩JS、CSS、image等前端资源(通常是由服务器来解决)
      程序的性能优化(如数据读写)
      采用CDN来加速资源加载
      对于JS DOM的优化
      HTTP服务器的文件缓存
### 3.列举IE与其他浏览器不一样的特性？
    * 触发事件的元素被认为是目标（target）。而在 IE 中，目标包含在 event 对象的 srcElement 属性；
    * 获取字符代码、如果按键代表一个字符（shift、ctrl、alt除外），IE 的 keyCode 会返回字符代码（Unicode），DOM 中    按键的代码和字符是分离的，要获取字符代码，需要使用 charCode 属性；
    * 阻止某个事件的默认行为，IE 中阻止某个事件的默认行为，必须将 returnValue 属性设置为 false，Mozilla 中，需要调用    preventDefault() 方法；
    * 停止事件冒泡，IE 中阻止事件进一步冒泡，需要设置 cancelBubble 为 true，Mozzilla 中，需要调用     stopPropagation()；
### 4.什么叫优雅降级和渐进增强？
    渐进增强 progressive enhancement：针对低版本浏览器进行构建页面，保证最基本的功能，然后再针对高级浏览器进行效果、交互等改进和追加功能达到更好的用户体验。
    优雅降级 graceful degradation：一开始就构建完整的功能，然后再针对低版本浏览器进行兼容。
    区别：优雅降级是从复杂的现状开始，并试图减少用户体验的供给，而渐进增强则是从一个非常基础的，能够起作用的版本开始，并不断扩充，以适应未来环境的需要。降级（功能衰减）意味着往回看；而渐进增强则意味着朝前看，同时保证其根基处于安全地带。
    “优雅降级”观点
    “优雅降级”观点认为应该针对那些最高级、最完善的浏览器来设计网站。而将那些被认为“过时”或有功能缺失的浏览器下的测试工作安排在开发周期的最后阶段，并把测试对象限定为主流浏览器（如 IE、Mozilla 等）的前一个版本。
    在这种设计范例下，旧版的浏览器被认为仅能提供“简陋却无妨 (poor, but passable)” 的浏览体验。你可以做一些小的调整来适应某个特定的浏览器。但由于它们并非我们所关注的焦点，因此除了修复较大的错误之外，其它的差异将被直接忽略。
    “渐进增强”观点
    “渐进增强”观点则认为应关注于内容本身。
    内容是我们建立网站的诱因。有的网站展示它，有的则收集它，有的寻求，有的操作，还有的网站甚至会包含以上的种种，但相同点是它们全都涉及到内容。这使得“渐进增强”成为一种更为合理的设计范例。这也是它立即被 Yahoo! 所采纳并用以构建其“分级式浏览器支持 (Graded Browser Support)”策略的原因所在。
### 5.是否了解公钥加密和私钥加密。
    一般情况下是指私钥用于对数据进行签名，公钥用于对签名进行验证;
    HTTP网站在浏览器端用公钥加密敏感数据，然后在服务器端再用私钥解密。
### 6.WEB应用从服务器主动推送Data到客户端有那些方式？
    * html5提供的Websocket
    * 不可见的iframe
    * WebSocket通过Flash
    * XHR长时间连接
    * XHR Multipart Streaming
    * <script>标签的长时间连接(可跨域)
### 7.你有用过哪些前端性能优化的方法？
    （1） 减少http请求次数：CSS Sprites, JS、CSS源码压缩、图片大小控制合适；网页Gzip，CDN托管，data缓存 ，图片服务器。
    （2） 前端模板 JS+数据，减少由于HTML标签导致的带宽浪费，前端用变量保存AJAX请求结果，每次操作本地变量，不用请求，减少请求次数
    （3） 用innerHTML代替DOM操作，减少DOM操作次数，优化javascript性能。
    （4） 当需要设置的样式很多时设置className而不是直接操作style。
    （5） 少用全局变量、缓存DOM节点查找的结果。减少IO读取操作。
    （6） 避免使用CSS Expression（css表达式)又称Dynamic properties(动态属性)。
    （7） 图片预加载，将样式表放在顶部，将脚本放在底部  加上时间戳。
    （8） 避免在页面的主体布局中使用table，table要等其中的内容完全下载之后才会显示出来，显示比div+css布局慢。
    对普通的网站有一个统一的思路，就是尽量向前端优化、减少数据库操作、减少磁盘IO。向前端优化指的是，在不影响功能和体验的情况下，能在浏览器执行的不要在服务端执行，能在缓存服务器上直接返回的不要到应用服务器，程序能直接取得的结果不要到外部取得，本机内能取得的数据不要到远程取，内存能取到的不要到磁盘取，缓存中有的不要去数据库查询。减少数据库操作指减少更新次数、缓存结果减少查询次数、将数据库执行的操作尽可能的让你的程序完成（例如join查询），减少磁盘IO指尽量不使用文件系统作为缓存、减少读写文件次数等。程序优化永远要优化慢的部分，换语言是无法“优化”的。
### 8.一个页面从输入 URL 到页面加载显示完成，这个过程中都发生了什么？（流程说的越详细越好）
    详细版：
      1.浏览器会开启一个线程来处理这个请求，对 URL 分析判断如果是 http 协议就按照 Web 方式来处理;
      2.调用浏览器内核中的对应方法，比如 WebView 中的 loadUrl 方法;
      3.通过DNS解析获取网址的IP地址，设置 UA 等信息发出第二个GET请求;
      4.进行HTTP协议会话，客户端发送报头(请求报头);
      5.进入到web服务器上的 Web Server，如 Apache、Tomcat、Node.JS 等服务器;
      6.进入部署好的后端应用，如 PHP、Java、JavaScript、Python 等，找到对应的请求处理;
      7.处理结束回馈报头，此处如果浏览器访问过，缓存上有对应资源，会与服务器最后修改时间对比，一致则返回304;
      8.浏览器开始下载html文档(响应报头，状态码200)，同时使用缓存;
      9.文档树建立，根据标记请求所需指定MIME类型的文件（比如css、js）,同时设置了cookie;
      10.页面开始渲染DOM，JS根据DOM API操作DOM,执行事件绑定等，页面显示完成。
    简洁版：
      1.浏览器根据请求的URL交给DNS域名解析，找到真实IP，向服务器发起请求；
      2.服务器交给后台处理完成后返回数据，浏览器接收文件（HTML、JS、CSS、图象等）；
      3.浏览器对加载到的资源（HTML、JS、CSS等）进行语法解析，建立相应的内部数据结构（如HTML的DOM）；
      4.载入解析到的资源文件，渲染页面，完成。
    注：这题胜在区分度高，知识点覆盖广，再不懂的人，也能答出几句，
        而高手可以根据自己擅长的领域自由发挥，从URL规范、HTTP协议、DNS、CDN、数据库查询、
        到浏览器流式解析、CSS规则构建、layout、paint、onload/domready、JS执行、JS API绑定等等；
### 9.常使用的库有哪些？常用的前端开发工具？开发过什么应用或组件？
    使用率较高的框架有jQuery、AngularJs, ReactJs, YUI、Prototype、Dojo、Ext.js、Mootools等。尤其是jQuery，超过91%。
    轻量级框架有Modernizr、underscore.js、backbone.js、Raphael.js等。（理解这些框架的功能、性能、设计原理）
    前端开发工具：WebStorm, Hbuilder, Sublime Text 、Eclipse、Notepad、Firebug、HttpWatch、Yslow。
    开发过的插件：城市选择插件，汽车型号选择插件、幻灯片插件。弹出层。（写过开源程序，加载器，js引擎更好）
### 10.平时如何管理你的项目？
    先期团队必须确定好全局样式（globe.css），编码模式(utf-8) 等；
    编写习惯必须一致（例如都是采用继承式的写法，单样式都写成一行）；
    标注样式编写人，各模块都及时标注（标注关键样式调用的地方）；
    页面进行标注（例如 页面 模块 开始和结束）；
    CSS跟HTML 分文件夹并行存放，命名都得统一（例如style.css）；
    JS 分文件夹存放 命名以该JS功能为准的英文翻译。
    图片采用整合的 images.png png8 格式文件使用 尽量整合在一起使用方便将来的管理
### 11.平时如何管理你的项目，如何设计突发大规模并发架构？
    先期团队必须确定好全局样式（globe.css），编码模式(utf-8) 等
    编写习惯必须一致（例如都是采用继承式的写法，单样式都写成一行）；
    标注样式编写人，各模块都及时标注（标注关键样式调用的地方）；
    页面进行标注（例如 页面 模块 开始和结束）；
    CSS跟HTML 分文件夹并行存放，命名都得统一（例如style.css）
    JS 分文件夹存放 命民以该JS 功能为准英文翻译；
    图片采用整合的 images.png png8 格式文件使用 尽量整合在一起使用方便将来的管理
### 12.那些操作会造成内存泄漏？
    先期团队必须确定好全局样式（globe.css），编码模式(utf-8) 等
    编写习惯必须一致（例如都是采用继承式的写法，单样式都写成一行）；
    标注样式编写人，各模块都及时标注（标注关键样式调用的地方）；
    页面进行标注（例如 页面 模块 开始和结束）；
    CSS跟HTML 分文件夹并行存放，命名都得统一（例如style.css）
    JS 分文件夹存放 命民以该JS 功能为准英文翻译；
    图片采用整合的 images.png png8 格式文件使用 尽量整合在一起使用方便将来的管理
### 13.你说你热爱前端，那么应该WEB行业的发展很关注吧？ 说说最近最流行的一些东西吧？
    Node.js、Mongodb、npm、react、angularjs、MVVM、MEAN
## 网络知识相关
### 1.http状态码
    简单版
    [
     100  Continue继续，一般在发送post请求时，已发送了http header之后服务端将返回此信息，表示确认，之后发送具体参数信息
     200  OK          正常返回信息
     201  Created   请求成功并且服务器创建了新的资源
     202  Accepted     服务器已接受请求，但尚未处理
     301  Moved Permanently  请求的网页已永久移动到新位置。
     302 Found          临时性重定向。
     303 See Other     临时性重定向，且总是使用 GET 请求新的 URI。
     304  Not Modified 自从上次请求后，请求的网页未修改过。
    
     400 Bad Request  服务器无法理解请求的格式，客户端不应当尝试再次使用相同的内容发起请求。
     401 Unauthorized 请求未授权。
     403 Forbidden    禁止访问。
     404 Not Found   找不到如何与 URI 相匹配的资源。
     500 Internal Server Error  最常见的服务器端错误。
     503 Service Unavailable 服务器端暂时无法处理请求（可能是过载或维护）。
    ]
     
    完整版
    1**(信息类)：表示接收到请求并且继续处理
    100——客户必须继续发出请求
    101——客户要求服务器根据请求转换HTTP协议版本
    2**(响应成功)：表示动作被成功接收、理解和接受
    200——表明该请求被成功地完成，所请求的资源发送回客户端
    201——提示知道新文件的URL
    202——接受和处理、但处理未完成
    203——返回信息不确定或不完整
    204——请求收到，但返回信息为空
    205——服务器完成了请求，用户代理必须复位当前已经浏览过的文件
    206——服务器已经完成了部分用户的GET请求
    3**(重定向类)：为了完成指定的动作，必须接受进一步处理
    300——请求的资源可在多处得到
    301——本网页被永久性转移到另一个URL
    302——请求的网页被转移到一个新的地址，但客户访问仍继续通过原始URL地址，重定向，新的URL会在response中的Location中返回，浏览器将会使用新的URL发出新的Request。
    303——建议客户访问其他URL或访问方式
    304——自从上次请求后，请求的网页未修改过，服务器返回此响应时，不会返回网页内容，代表上次的文档已经被缓存了，还可以继续使用
    305——请求的资源必须从服务器指定的地址得到
    306——前一版本HTTP中使用的代码，现行版本中不再使用
    307——申明请求的资源临时性删除
    4**(客户端错误类)：请求包含错误语法或不能正确执行
    400——客户端请求有语法错误，不能被服务器所理解
    401——请求未经授权，这个状态代码必须和WWW-Authenticate报头域一起使用
    HTTP 401.1 - 未授权：登录失败
    　　HTTP 401.2 - 未授权：服务器配置问题导致登录失败
    　　HTTP 401.3 - ACL 禁止访问资源
    　　HTTP 401.4 - 未授权：授权被筛选器拒绝
    HTTP 401.5 - 未授权：ISAPI 或 CGI 授权失败
    402——保留有效ChargeTo头响应
    403——禁止访问，服务器收到请求，但是拒绝提供服务
    HTTP 403.1 禁止访问：禁止可执行访问
    　　HTTP 403.2 - 禁止访问：禁止读访问
    　　HTTP 403.3 - 禁止访问：禁止写访问
    　　HTTP 403.4 - 禁止访问：要求 SSL
    　　HTTP 403.5 - 禁止访问：要求 SSL 128
    　　HTTP 403.6 - 禁止访问：IP 地址被拒绝
    　　HTTP 403.7 - 禁止访问：要求客户证书
    　　HTTP 403.8 - 禁止访问：禁止站点访问
    　　HTTP 403.9 - 禁止访问：连接的用户过多
    　　HTTP 403.10 - 禁止访问：配置无效
    　　HTTP 403.11 - 禁止访问：密码更改
    　　HTTP 403.12 - 禁止访问：映射器拒绝访问
    　　HTTP 403.13 - 禁止访问：客户证书已被吊销
    　　HTTP 403.15 - 禁止访问：客户访问许可过多
    　　HTTP 403.16 - 禁止访问：客户证书不可信或者无效
    HTTP 403.17 - 禁止访问：客户证书已经到期或者尚未生效
    404——一个404错误表明可连接服务器，但服务器无法取得所请求的网页，请求资源不存在。eg：输入了错误的URL
    405——用户在Request-Line字段定义的方法不允许
    406——根据用户发送的Accept拖，请求资源不可访问
    407——类似401，用户必须首先在代理服务器上得到授权
    408——客户端没有在用户指定的饿时间内完成请求
    409——对当前资源状态，请求不能完成
    410——服务器上不再有此资源且无进一步的参考地址
    411——服务器拒绝用户定义的Content-Length属性请求
    412——一个或多个请求头字段在当前请求中错误
    413——请求的资源大于服务器允许的大小
    414——请求的资源URL长于服务器允许的长度
    415——请求资源不支持请求项目格式
    416——请求中包含Range请求头字段，在当前请求资源范围内没有range指示值，请求也不包含If-Range请求头字段
    417——服务器不满足请求Expect头字段指定的期望值，如果是代理服务器，可能是下一级服务器不能满足请求长。
    5**(服务端错误类)：服务器不能正确执行一个正确的请求
    HTTP 500 - 服务器遇到错误，无法完成请求
    　　HTTP 500.100 - 内部服务器错误 - ASP 错误
    　　HTTP 500-11 服务器关闭
    　　HTTP 500-12 应用程序重新启动
    　　HTTP 500-13 - 服务器太忙
    　　HTTP 500-14 - 应用程序无效
    　　HTTP 500-15 - 不允许请求 global.asa
    　　Error 501 - 未实现
    HTTP 502 - 网关错误
    HTTP 503：由于超载或停机维护，服务器目前无法使用，一段时间后可能恢复正常
### 2.http报头有哪些
    请求头： 
    Accept
    Cache-control
    Host
    User-agent
    Accenp-Language
    
    响应头：
    Cache-Control:max-age 避免了服务端和客户端时间不一致的问题。
    content-type
    Date
    Expires
    Last-Modified 标记此文件在服务期端最后被修改的时间
    httpOnly 设置cookie是否能通过 js 去访问。在客户端是不能通过js代码去设置一个httpOnly类型的cookie的，这种类型的cookie只能通过服务端来设置。在发生跨域时，cookie 作为一种 credential 信息是不会被传送到服务端的。必须要进行额外设置才可以。
### 3.代理和反向代理
    正向代理： 用浏览器访问时，被残忍的block，于是你可以在国外搭建一台代理服务器，让代理帮我去请求google.com，代理把请求返回的相应结构再返回给我。
    反向代理：反向代理服务器会帮我们把请求转发到真实的服务器那里去。Nginx就是性能非常好的反向代理服务器，用来做负载均衡。 正向代理的对象是客户端，反向代理的对象是服务端
### 4.网络优化/性能优化
    使用CDN，让用户访问最近的资源，减少来回传输时间 合并压缩CSS、js、图片、静态资源，服务器开启GZIP css放顶部，js放底部（css可以并行下载，而js加载之后会造成阻塞） 图片预加载和首屏图片之外的做懒加载 做HTTP缓存（添加Expires头和配置Etag）用户可以重复使用本地缓存，减少对服务器压力 大小超过 10KB 的 css/img 建议外联引用，以细化缓存粒度 小于 10k 的图片 base64 DNS 预解析 DNS-Prefetch 预连接 Preconnect
    1.代码层面优化
    少用全局变量，减少作用域链查找，缓存DOM查找结果，避免使用with（with会创建自己的作用域，会增加作用域链长度）；多个变量声明合并；减少DOM操作次数；尽量避免在HTML标签中写style属性 
    避免使用css3渐变阴影效果，尽量使用css3动画，开启硬件加速，不滥用float；避免使用CSS表达式；使用<link>来代替@import
    2.使用图片预加载技术减少资源请求
### 5.首屏优化
    总结以下可以通过以下三步解决
    分拆打包 现在流行的路由库如 react-router 对分拆打包都有很好的支持。可以按照页面对包进行分拆，并在页面切换时加上一些 loading 和 transition 效果。
    1.首屏内容最好做到静态缓存 2.首屏内联css渲染 3.图片懒加载 4.服务端渲染，首屏渲染速度更快（重点），无需等待js文件下载执行的过程 5.交互优化（使用加载占位器，在白屏无法避免的时候，为了解决等待加载过程中白屏或者界面闪烁） 6.图片尺寸大小控制
    前端渲染的优势
    1. 局部刷新。无需每次都进行完整页面请求
    2. 懒加载。如在页面初始时只加载可视区域内的数据，滚动后rp加载其它数据，可以通过 
    3. react-lazyload 实现
    4. 富交互。使用 JS 实现各种酷炫效果
    5. 节约服务器成本。省电省钱，JS 支持 CDN 部署，且部署极其简单，只需要服务器支持静态文件即可
    6. 天生的关注分离设计。服务器来访问数据库提供接口，JS 只关注数据获取和展现
    7. JS 一次学习，到处使用。可以用来开发 Web、Serve、Mobile、Desktop 类型的应用
### 6.服务端渲染的优势
    1. 更好的 SEO，由于搜索引擎爬虫抓取工具可以直接查看完全渲染的页面。
    2. 服务端渲染不需要先下载一堆 js 和 css 后才能看到页面（首屏性能）
    3. 服务端渲染不用关心浏览器兼容性问题（随意浏览器发展，这个优点逐渐消失）
    4. 对于电量不给力的手机或平板，减少在客户端的电量消耗很重要
### 7.为什么利用多个域名来存储网站资源会更有效？
    CDN缓存更方便
    突破浏览器并发限制
    节约cookie带宽
    节约主域名的连接数，优化页面响应速度
    防止不必要的安全问题
### 8.请谈一下你对网页标准和标准制定机构重要性的理解。
    网页标准和标准制定机构都是为了能让web发展的更‘健康’，开发者遵循统一的标准，降低开发难度，开发成本，SEO也会更好做，也不会因为滥用代码导致各种BUG、安全问题，最终提高网站易用性。
### 9.前端开发的优化问题。
    （1） 减少http请求次数：CSS Sprites, JS、CSS源码压缩、图片大小控制合适；网页Gzip，CDN托管，data缓存 ，图片服务器。
    （2） 前端模板 JS+数据，减少由于HTML标签导致的带宽浪费，前端用变量保存AJAX请求结果，每次操作本地变量，不用请求，减少请求次数
    （3） 用innerHTML代替DOM操作，减少DOM操作次数，优化javascript性能。
    （4） 当需要设置的样式很多时设置className而不是直接操作style。
    （5） 少用全局变量、缓存DOM节点查找的结果。减少IO读取操作。
    （6） 避免使用CSS Expression（css表达式)又称Dynamic properties(动态属性)。
    （7） 图片预加载，将样式表放在顶部，将脚本放在底部  加上时间戳。
    （8） 避免在页面的主体布局中使用table，table要等其中的内容完全下载之后才会显示出来，显示比div+css布局慢。
### 10.网页验证码是干嘛的，是为了解决什么安全问题。
     区分用户是计算机还是人的公共全自动程序。可以防止恶意破解密码、刷票、论坛灌水；
      有效防止黑客对某一个特定注册用户用特定程序暴力破解方式进行不断的登陆尝试。
# 晨测题集合
## 2019-03-05
### 1. 内存,数据, 变量三者之间的关系
```
内存是可以存储不同数据的容器
变量与内存对应, 变量名是内存的标识名称, 变量值是内存中保存的数据
```
### 2. 关于引用变量赋值的2个问题
```
2个引用变量指向同一个对象, 通过一个引用变量修改对象内部数据, 另一个引用变量也看得见
2个引用变量指向同一个对象,让一个引用变量指向另一个对象, 另一个引用变量还是指向原来的对象
```
### 3. 如何判断函数是否是回调函数
```
你定义的
你没有直接调用
但它最终执行了
```
### 4. 说说对象,数组, 函数的区别
```
对象: 一个可以存储多个无序数据的容器
数组: 一个特别的对象, 存储的数据是有序的
函数: 一个特别的对象, 包含可执行的代码(可以执行) 
```
```javascript
var p1 = {id: 1, name: 'Tom'}
var p2 = {id: 2, name: 'Tom2'}
var p3 = {id: 3, name: 'Tom'}

var obj = {}
obj[p1.id] = p1
obj[p2.id] = p2
obj[p3.id] = p3

var arr = []
arr.push(p1)
arr.push(p2)
arr.push(p3)

var id = 3
arr.forEach (function (p){if(p.id===id){}})
obj[id]
```
### 5. 如何判断函数中的this
```
直接调用: xxx()  : window
new调用: new xxx() : 新创建的对象
对象调用: obj.xxx() : obj
call/apply调用: xxx.call(obj) : obj
```
## 2019-03-06
### 6. git的6个基本操作
```
1). 创建本地仓库
    创建.gitignore并配置忽略
    git init
    git add *
    git commit -m "xxx"
2). 创建远程仓库
    New Repository
    指定名称
    创建
3). 将本地仓库推送到远程仓库
    git remote add origin https://github.com/zxfjd3g/xxx.git 关联远程仓库
    git push origin master
4). 如果本地有更新, 推送到远程
    git add *
    git commit -m "xxx"
    git push origin master
5). 如果远程有更新, 拉取到本地
    git pull origin master
6). 克隆远程仓库到本地
    git clone https://github.com/zxfjd3g/xxx.git
```
### 7. 说说原型与原型链
```
1). 所有函数都有prototype属性(显式原型), 属性值指向原型对象
2). 所有实例对象都有__proto__属性(隐式原型), 其值就就等于构造函数的prototytpe属性值, 也就是也指向原型对象
3). 多个对象通过__proto__属性就形成了链就是原型链, 在查找对象的某个属性时: 先在对象自身上找, 如果找不到就会沿着原型链查找
```
```javascript
function Foo () {}
var f1 = new Foo()
var f2 = new Foo()
var o1 = {}
var o2 = {}

var obj1 = {m: 1}
var obj2 = {n: 2}
obj1.xxx = obj2

var parent = {name: 'tom'}
var child = {name: 'jack'}
child.parent = parent
var child2 = {name: 'bob'}
child2.parent = parent
child2.parent.name
parent.children = []
parent.children.push(child)
parent.children.push(child2)

parent.children.forEach(function (child) {child.name})

a.b
```
### 8. 说说变量提升和函数提升
```
1). 变量提升: 在用var声明变量的语句之前就可以访问到变量 ==> var声明变量部分被提前执行了
2). 函数提升: 在function函数声明语句之前就可以调用函数 ==> 函数声明语句被提前执行了
3). 原因: JS引擎在执行代码时做了预处理/解析的工作
```
### 9. 说说作用域与作用域链
```
1). 作用域: 变量可见的一块特定代码区域, 包括全局作用域和函数作用域, 作用域在编写代码时就确定了
2). 作用域链: 由内向外的多个嵌套作用域形成的链, 在查找变量时, 会沿着作用域链查找
```
### 10. 画内存详图
![原型链图](https://s2.ax1x.com/2019/03/14/AA3sqs.png)
## 2019-03-07
### 11. 产生闭包的条件与什么是闭包
```
1). 产生闭包的条件
    * 函数嵌套
    * 内部函数引用了外部函数的数据(变量/函数)
    * 执行外部函数
2). 什么是闭包
    闭包本质是内部函数中的一个容器, 这个容器中包含引用的变量
```
### 12. 编写程序演示闭包
```javascript
function fn1() {
  var a = 3
  function fn2() {
    a++
    console.log(a)
  }
  return fn2
}
var f = fn1()
f() // 4
f() // 5
```
### 13. 对象创建模式(4种)
```javascript
//1). Object构造函数模式
  var obj = {}
  obj.name = 'Tom'
  obj.setName = function(name){this.name=name}
//2). 对象字面量模式
  var obj = {
    name : 'Tom',
    setName : function(name){this.name = name}
  }
//3). 构造函数模式
  function Person(name, age) {
    this.name = name
    this.age = age
    this.setName = function(name){this.name=name}
  }
  new Person('tom', 12)
//4). 构造函数+原型的组合模式
  function Person(name, age) {
    this.name = name
    this.age = age
  }
  Person.prototype.setName = function(name){this.name=name}
  new Person('tom', 12)
```
### 14. 编码实现组合继承模式
```javascript
function Parent(xxx){this.xxx = xxx}
Parent.prototype.test = function(){}
function Child(xxx,yyy){
  Parent.call(this, xxx) //借用构造函数   this.Parent(xxx)
}
Child.prototype = new Parent() //得到test()
Child.proptotype.constructor = Child
Child.prototype.test2 = function (){}
var child = new Child() //child.xxx为'a', 也有test()
```
### 15. 下面代码的输出
```javascript
for (var i = 0; i < 3; i++) {
    setTimeout(function() {
        console.log(i);
    }, 0);
    console.log(i);
} // 0 1 2 3 3 3

for (var i = 0; i < 5; i++) { 
    (function(i){
        setTimeout(function (){
            console.log(i);  
         },1000);  
    })(i);  
} // 0 1 2 3 4
```
## 2019-03-08
### 16.简述一下js有几种数据类型？
```
//1.基础数据类型：（简记：nubnss）
null，undefined，boolean，number，string，symbol

//2.对象(引用，复合)类型
Object，Array，Function
```
### 17.typeof的返回值有几种？typeof有什么特点？
```
>Typeof：一个不正确，两个不精确，其余都准确。
>一个不正确：typeof(null)的结果是object。
>两个不精确：typeof []和{} 的结果都是object
```
```
typeof 1 // 'number'
typeof '1' // 'string'
typeof undefined // 'undefined'
typeof true // 'boolean'
typeof console.log // 'function'
typeof Symbol() // 'symbol'
typeof b // b 没有声明，但是还会显示 undefined
typeof [] // 'object'
typeof {} // 'object'
typeof null // 'object'
```
### 18.说一下null与undefined？
```
>undefined：没有赋值（一般不会将一个变量手动赋值为undefined，因为没有意义）
	var a;
	console.log(a)//undefined
	console.log(b)//undefined
	var b = 1;

>null：赋值了, 但是值为null	
	//一个约定俗称的小规矩，一般赋值为null代表这个变量接下来要存储的类型是对象
	var a = null 
```
### 19.说出下列语句的结果
```
console.log(1 + '1')//'11'
console.log('1'- 0)//1
console.log(2 * '2')//4
console.log([1, 2] + [2, 1])//1,22,1
console.log(typeof console.log('1'- 0))// 1 undefined
```
### 20.控制台输出什么？
```
var obj = {
  fn2: function () {
     console.log(fn2)
    // 如果想输出fn2属性的值，这么写：console.log(this.fn2) 或者 obj.fn2才行。
  }
}
obj.fn2() //报错， fn2 is not defined
```
## 2019-03-09
### 21.写出以下代码段的执行结果
```
var a = 2
function fn() {
  console.log(a)
  var a = 3
}
fn() // undefined
```
### 22.写出以下代码段的执行结果
```javascript
var c
function c(c) {
 console.log(c)
 var c = 3
}
c = 1
console.log(c) // 1
c(2) //报错  c is not a function

/*
* 解析：
* 1.预处理的结果：c是function。
* 2.正常执行代码，跳过function c（因为已经与解析完），后来c变成了1
* 3.打印c是1，执行1，1明显不是函数，报错
*
* */
```
### 23.写出以下代码段的执行结果
```javascript
var c
function c(c) {
  console.log(c)
  c = 3
}
console.log(c) // 函数体
c(2) //2
```
### 24.写出控制台输出结果
```javascript
var obj = {
  fn2: function () {
     console.log(fn2)
    // 如果想输出fn2属性的值，这么写：console.log(this.fn2) 或者 obj.fn2也行, 但比较死。
  }
}
obj.fn2() //报错， fn2 is not defined
```
### 25.写出控制台输出结果
```
function fn() {
  console.log(a)
  var a = 3
}
fn() // undefined
//解析：函数内部预处理，不去找全局的a，同时变量提升，打印之前a定义了，还未赋值，undefined
```
## 2019-03-11
### 26.写出控制台输出结果
```javascript
function fn2() { // 预计处理:
  console.log(a)
  a = 3  // 此时的a是全局变量
}
fn2() // a is not defined
```
### 27.你所知道的常见拷贝技术？都分别都有什么问题或特点？
```
1. arr.concat(): 数组浅拷贝
2. arr.slice(): 数组浅拷贝
3. JSON.parse(JSON.stringify(arr/obj)): 数组或对象深拷贝, 但不能处理函数数据
```
### 28.请编写一个函数，完成深度复制对象类型的数据
```javascript
//判断数据类型的方法
function getTYpe(target) {
  return Object.prototype.toString.call(target).slice(8,-1)
}
function clone(target) {
  let result
  if(getTYpe(target) === 'Object'){
    result = {}
  }
  else if(getTYpe(target) === 'Array'){
    result = []
  }
  else{
    return target
  }
  
  for (let key in target){
     let item = target[key]
      if(getTYpe(item) === 'Object'||'Array'){
        result[key] = clone(item)
      }
      else{
        result[key] = item
        }
    }
  return result
  }
```
### 29.写出控制台输出结果
```javascript
var name = 'World!';
(function () {
  if (typeof name === 'undefined') {
    var name = 'Jack'
    console.log(name)
  } else {
    console.log(name)
  }
})()
//最终输出：Jack
/*
* 解析：
* 1.预处理结果，外部name是undefined，内部name是undefined。
*   这里值得注意的是：不管if判断是否为真，内部的name都会在预解析的时候变为undefined，ES6之前不存在块级作用域。
* 2.执行代码：外部name是World，内部是undefined。
* 3.进入if判断，内部name为Jack。
* */
```
### 30.写出控制台输出结果
```javascript
var a = 6
setTimeout(function () {
  console.log(0)
  console.log(a)
  a = 666
}, 0)
console.log(1)
a = 66

/*
* 解析：
* 1.虽然setTimeout的时间是0，但是浏览器的循环，所有的事件管理模块，都会在等当前主线程代码运行完毕后
*   事件管理模块才根据自己的触发条件，将自己管理的事件放在执行队列里，所以是0，也要等着！等着console.log(1)
*   和a = 66全部执行完。
* 2.最终结果是：控制台打印：1，0，66。
* */
```
## 2019-03-12
### 31.写出控制台输出结果 
```javascript
var obj = { 
    fn2: function () { 
        console.log(fn2) 
    } 
  } 
obj.fn2()
```
### 32.写出控制台输出结果
```javascript
function fn() {
    console.log(a)
    var a = 3
  }
fn()
```
### 33.写出控制台输出结果
```javascript
function fn2() { 
    console.log(a)
    a = 3 
}
fn2() 
```
### 34.你所知道的常见拷贝技术？都分别都有什么问题或特点？
```
常用的拷贝技术
1). arr.concat(): 数组浅拷贝
2). arr.slice(): 数组浅拷贝
3). JSON.parse(JSON.stringify(arr/obj)): 数组或对象深拷贝, 但不能处理函数数据
4). 浅拷贝包含函数数据的对象/数组
5). 深拷贝包含函数数据的对象/数组
```
### 35.请编写一个函数，完成深度复制对象类型的数据
```javascript
//判断数据类型的方法
function getTYpe(target) {
  return Object.prototype.toString.call(target).slice(8,-1)
}

/**
 * 深度复制的方法
 * @param target
 * @returns {*}
 */
function clone(target) {
  let result
  if(getTYpe(target) === 'Object'){
    result = {}
  }
  else if(getTYpe(target) === 'Array'){
    result = []
  }
  else{
    return target
  }
  
  for (let key in target){
     let item = target[key]
      if(getTYpe(item) === 'Object'||'Array'){
        result[key] = clone(item)
      }
      else{
        result[key] = item
      }
  }
return result
}
```
## 2019-03-13
### 36.说说你所知道的js模块化技术
```
1. CommonJS（后端常用）
	2. AMD
	3. CMD
	4. ES6（前端常用）
```
### 37.说出以下代码执行结果
```javascript
function fn1() {
  var a = 2
  function fn2 () {
    a++
    console.log(a)
  }
  return fn2
}
var f = fn1()
f()
f()
// 3
// 4
```
### 38.说出以下代码执行结果
```javascript
var name = "A"
var object = {
  name: "B",
  getNameFunc: function () {
    return function () {
      return this.name 
    }
  }
}
console.log(object.getNameFunc()())  //A
/*
* 1. object.getNameFunc()的this是object，返回了1个function
* 2. 再次调用返回的object时，此时是函数直接调用，this是window
* */
```
### 39.请说说node.js有哪些特点？
```
1) 异步非阻塞的I/O（I/O线程池）
2) 特别适用于I/O密集型应用
3) 单线程
4) 处理不好CPU密集型服务
```
### 40.说说你对Node中的Buffer缓冲器的了解
```
1.它是一个类数组的对象，用于存储数据（存储的全部是二进制数据）
2.Buffer的效率很高，存储和读取的速度都很快，本质就是对内存的直接操作
3.Buffer的大小一旦确定了，不可修改。
4.每个元素占用内存的空间是1字节。
5.Buffer是Node中非常核心的模块，无需下载，无需引入，直接可以使用
```
## 2019-03-16
### 41.说出程序执行结果
```javascript
function Person() {
  getAge = function () {
    console.log(10)
  }
  return this
}
Person.getAge = function () {
  console.log(20)
}
Person.prototype.getAge = function () {
  console.log(30)
}
var getAge = function () {
  console.log(40)
}
function getAge() {
  console.log(50)
}
Person.getAge() 
getAge()
Person().getAge() 
getAge()
new Person.getAge() 
new Person().getAge() 
/*  Person.getAge() //20 把Person看成对象，调用对象里的一个方法，执行Person.getAge = function ()...
    getAge() //40 执行全局的getAge，全局有两个，不过用var声明的那个是有效的.
    Person().getAge() //10 先执行Person()，执行的结果是:全局的getAge被重写，返回的this是window，执行35行（此时已被覆盖）
    getAge() //10 执行全局的那个，已经被覆盖了，是10
    new Person.getAge() //20 Person是对象，执行对象.方法，输出20
    new Person().getAge() //30 new Person()是Person的一个实例，实例调用getAge，自己没有找原型，找到了，30.*/
```
### 42.请说出搭建原生Node服务的几个关键步骤
```javascript
//1.引入http模块
let http = require('http')
//2.创建一个服务对象
let server = http.createServer((request,response)=>{
  //返回响应
  response.end('<h3>这是我的第一个原生node服务器</h3>')
})
//3.绑定端口号
server.listen(3000,(err)=>{
  if (!err) console.log('服务器启动成功了,访问地址是：http://localhost:3000')
  else console.log('服务器启动失败，'+err)
})
```
### 43.简要说一下你对MongoDB的了解
```
1.它是最具代表性的非关系行数据库（NoSQL）。
2.MongoDB是文档结构的存储方式，类JSON数据模式，能够更便捷的获取数据。
3.它不像关系型统数据库有严格的字段约束，更加灵活多变。
4.不支持事务（事务不可分割，具有原子性），一般关系型数据库都支持事务。  
```
## 2019-03-18
### 44.说出常见的http状态码及含义
```
http状态码的分类
1xx : 服务器已经收到了本次请求，但是还需要进一步的处理才可以。
2xx : 服务器已经完整的接收到了客户端的请求，并且已经进入到：分析、理解.....最终处理完毕
3xx : 服务器已经接收到了请求，还需要其他的资源，或者指定新的东西返回，甚至交给其他服务器处理
4xx ：一般指请求的参数或者地址有错误， 出现了服务器无法理解的请求（前端的锅）
5xx ：服务器内部错误（不是因为请求地址或者请求参数不当造成的），无法响应用户请求（后端人员的锅）
常见的几个状态码
200 ：成功
302 ：请求地址的重定向，服务器将你的请求重新定向到一个新的地址，搜索引擎会保留旧地址里面的内容
304 ：请求资源重定向到缓存中
404 ：资源未找到
500 ：服务器内部错误
502 ：连接服务器失败（服务器在处理一个请求的时候，或许需要其他的服务器配合，但是联系不上其他的服务器了）
```
### 45.说说GET请求和POST请求的区别

**主要区别：**

 区别 | GET | POST 
-|-|-
后退按钮/刷新 | 无害 | 数据会被重新提交（浏览器应该告知用户数据会被重新提交）。
书签 | 可收藏为书签 | 不可收藏为书签
缓存 | 能被缓存 | 不能缓存
编码类型 | application/x-www-form-urlencoded | application/x-www-form-urlencoded 或 multipart/form-data。为二进制数据使用多重编码。
历史 | 参数保留在浏览器历史中。 |参数不会保存在浏览器历史中。
对数据长度的限制 | 当发送数据时，GET 方法向 URL 添加数据；URL 的长度是受限制的（URL 的最大长度是 2048 个字符）。| 无限制。
对数据类型的限制 | 只允许 ASCII 字符。| 没有限制。也允许二进制数据。
安全性 | 与 POST 相比，GET 的安全性较差，因为所发送的数据是 URL 的一部分。在发送密码或其他敏感信息时绝不要使用 GET ！|POST 比 GET 更安全，因为参数不会被保存在浏览器历史或 web 服务器日志中。
可见性|数据在 URL 中对所有人都是可见的。|数据不会显示在 URL 中。


### 46.说说你所了解的express中间件

> 什么是中间件？:

1. Express 是一个自身功能极简，完全是由路由和中间件构成一个的 web 开发框架：从本质上来说，一个 Express 应用就是在调用各种中间件。
2. 中间件（Middleware） 是一个函数，它可以访问请求对象（request）, 响应对象（response）, 和 web 应用中处于请求-响应循环流程中的中间件，一般被命名为 next 的变量。

> 中间件功能:

1. 执行任何代码。
2. 修改请求和响应对象。
3. 终结请求-响应循环。
4. 调用堆栈中的下一个中间件。


### 47.说一下从用户输入url到用户看到页面的过程，都发生了什么？
```
> 一、DNS解析（优先找缓存）
    1.找浏览器的DNS缓存去解析域名（在chrome浏览器地址栏里输入：chrome://chrome-urls）
    2.找本机（电脑）的DNS缓存去解析域名:ipconfig/displaydns）
    3.找到路由器DNS缓存
    4.找到运营商DNS缓存（绝大多数的DNS查询到此为止）
    5.进行递归查询，全球13台的根DNS服务器（不存在查不到）
> 二、建立TCP（协议）连接，三次握手（根据上一次解析回来的ip地址，去联系服务器）
    第一次握手：由浏览器发给服务器，（我想和你说话，你能听见嘛？）
    第二次握手：由服务器发给浏览器，（我能听见，你说吧）
    第三次握手：由浏览器发给服务器，（好，那你听着吧，我开始说话了）
> 三、发送请求（请求报文）
> 四、得到响应（响应报文）
> 五、浏览器开始解析html文件
--预解析：将外部资源请求，发出去
--解析html：生成了DOM树
--解析css：生成CSSOM树
--合成一个render（渲染）树
--js是否操作了DOM？
    --操作了：进行重绘重排（不好，1.尽量避免 2.最小化）
    --没有操作：null
> 六、断开TCP连接，四次挥手
第一次挥手：由浏览器发给服务器（我的数据接收完了，你关闭吧）
第二次挥手：由服务器发给浏览器（1：你等等，我还有一些东西没接受完；2：你等等，我再检查一遍，数据是否完整）
第三次挥手：由服务器发给浏览器（1：我的数据接收完了，你断开吧；2：我检查完毕了，数据是完整的，你断开吧）
第四次挥手：由浏览器发给服务器（好的，我断开了）
```
## 2019-03-19
### 48.说说你对cookie的理解
1. 它的本质上就是一个浏览器所存储的字符串，里面包含着客户端和服务器交互时所产生的信息，
2. 它分为：会话cookie、持久化cookie，他们的保存位置不一样
3. 工作原理：
	   -当浏览器第一次请求服务器时，服务器可能会响应一个cookie，包裹在响应报文中。
       -当浏览器收到这个cookie之后，将这个cookie保存起来。
           --如果是会话cookie，保存在浏览器的那一块内存中。
           --如果是持久化的cookie，保存在用户的硬盘里。
       -当浏览器再一次请求服务器时（还是刚刚的那台服务器），自动携带之前所有该网站的cookie
       -服务器接收到cookie之后，查看cookie里的内容，根据cookie里的内容，进行一些逻辑处理。
4. 早期用于解决http无状态的问题，现在一般配合session一起解决
5. 在不同的语言、不同的框架中，操作cookie的语法是不一样的，工作的原理和过程是一样的。
### 49.请写出使用express搭建服务器的关键步骤
```javascript
//引入express模块
let express = require('express')
//实例化一个app应用对象
let app = express()

app.get('/demo',(req,res)=>{
  res.send('ok')
})

//绑定端口监听
app.listen(3000,(err)=>{
  if(!err) console.log(`服务器成功启动了，访问地址是：http://localhost:${PORT}`)
  else console.log('服务器启动失败，'+err)
})
```
### 50.说说express中路由器的基本使用步骤：
```javascript
//引入express
let express = require('express')
//引入路由器构造函数
let {Router} = express
//实例一个路由器对象
let router = new Router()

router.get('/demo',(req,res)=>{
  res.send('demo')
})
//暴露路由器
module.exports = router
```
## 2019-03-20
### 51.对cookie和session有了解吗？简单说一说
#### 关于cookie：
```
1.是什么？（早期用于解决http无状态的问题）
-本质上就是一个浏览器所存储的字符串，里面包含着客户端和服务器交互时所产生的信息，
-存储的形式是：key-value的形式
-浏览器请求服务器时会自动携带目标网站的cookie
-cookie一般是由服务器生成（“种”），随着第一次响应返回给客户端
-前端人员（通过js）一样可以生成cookie，但是意义不大。
-一般来说网站不会只用cookie去存储信息，一般还会配合session
2.分类：
-会话cookie：保存在运行浏览器的那一块内存中（关闭浏览器之后，cookie自动消失）
-持久化cookie：保存在用户的硬盘里，直到达到了过期的时间，才自动销毁。
3.工作原理：
-当浏览器第一次请求服务器时，服务器可能会响应一个cookie，包裹在响应报文中。
-当浏览器收到这个cookie之后，将这个cookie保存起来。
   --如果是会话cookie，保存在浏览器的那一块内存中。
   --如果是持久化的cookie，保存在用户的硬盘里。
-当浏览器再一次请求服务器时（还是刚刚的那台服务器），自动携带之前所有该网站的cookie
-服务器接收到cookie之后，查看cookie里的内容，根据cookie里的内容，进行一些逻辑处理。
4.应用：
 -早期用于解决http无状态的问题
 -七天免登录等等
5.在不同的语言、不同的框架中，操作cookie的语法是不一样的，工作的原理和过程是一样的。
备注：
在express框架中，不需要任何的第三方插件（库）就可以实现设置cookie
```
#### 关于session
```
1.是什么？
标准来说，session指的是会话，但是我们常说的session，全称叫做session会话存储。
2.特点：
 1.存在于服务端（相对cookie稍安全一些）
 2.存储的是浏览器（客户端）和服务器沟通产生的一些数据
3.默认session存储于服务器的运行内存中，当服务器开启session会话存储之后，没来一次新的请求
都会开辟一块内存空间，用来保存当前会话所产生的数据（例子中的：盒子）。
4.工作流程：
--第一次请求时，服务器会开辟一块内存空间，用于保存浏览器和服务器交互产生的数据
  将这一块空间内放置一个会话存储器（开启了一个session会话存储）并且对其进行编号。
--返回响应的时候，会自动给浏览器一个cookie，cookie里存储的是上一步session会话存储的编号
--第二次去请求服务器的时候，会自动携带着该网站cookie。
--服务器解析cookie里的内容，去和服务器所保存的session编码比对,从而进一步处理请求
备注：
 1.一般来说session都配合这cookie使用
 2.一般的服务器都会做session持久化，（避免因重启服务器导致用户还需要再次登录
```
#### cookie和session的区别:
```
他们都是用于解决http无状态的问题，并且一般配合着使用
储存位置
  cookie保存在浏览器中(不是很安全,别人可以分析存放在本地的cookie并进行cookie欺骗)
  session保存在服务器中(相对安全)
大小容量
  cookie 4kb 20个左右
  session 理论上没有限制
传输流量
  cookie每次请求时都会自动携带上，cookie越多，流量越大
  session会产生一个cookie（包含session“容器”的编号），所有数据都在服务器端，流量相对较小
一般来说网站为了安全，会在上述cookie的基础上，返回一些其他cookie用于校验用户身份
```
### 52.对js的原生Ajax是否了解？请写一下使用原生JS发送Ajax请求的核心代码
```javascript
//1.创建XMLHttpRequest对象，一般命名xhr
    let xhr = new XMLHttpRequest()
//2.绑定一个监听
xhr.onreadystatechange = function () {
  if(xhr.readyState === 4 && xhr.status === 200){
    console.log(xhr.responseText.length)
  }
}
//3.设置请求信息（请求方式、请求地址、请求参数）
xhr.open('GET','http://localhost:3000/demo1?name=kobe')
//设置请求头信息
xhr.setRequestHeader('content-type','text/plain')//get请求不常用
//4.发送请求
xhr.send()
```
### 53.原生Ajax中如何取消上一次请求？在你之前写过的项目中，什么时候需要取消上一次Ajax请求？
```
1. 使用abort()方法即可取消本次请求，我们可以缓存上一次请求的XMLHttpRequest实例对象，然后在下一次请求时，调用之前XMLHttpRequest实例对象上的abort()方法即可取消上次请求。
2. 例如验证码逻辑，如果用户短时间内发出多个请求获取验证码，就需要取消上一次请求。
```
### 54.写一下用jquery发送Ajax请求的核心代码
```javascript
$.ajax('http://localhost:3000/demo1',{
  method:'GET',
  data:{
    name:'kobe',
    age:12
  },
  success:function (data) {
    console.log(data)
  },
  error:function (err) {
    console.log(err)
  }
})
```
### 55.在之前的项目中，你都用过哪些发送Ajax库？
## 2019-03-22
### 56.请说几个开发中常用的ES6新语法
```
定义变量/常量: const/let
解构赋值: let {a, b} = this.props / import {aa} from 'xxx' / function f ({name}) {}
对象的简洁表达: {a, b, c () {}}
箭头函数: 
   优点:
      * 简洁
      * 没有自己的this,使用引用this查找的是外部this
扩展运算符: ...
类: class/extends/constructor/super
ES6模块化: export/default/import
异步: promise, async/await
```
### 57.说说你对闭包的了解
```
1. 如何产生闭包?
   2个函数嵌套
   内部函数引用了外部函数内的局部变量
   执行外部函数
2. 是什么?
   包含了那个局部变量的容器
   它被内部函数对象引用着
3. 作用?
   延长局部变量的生命周期
   使函数外部可以多次间接操作到函数内部的数据
4. 应用?
   循环遍历加监听
   IIFE定义模块
   jQuery内部
5. 代码演示:
	   function fn1 () {
	      
	      var a = 2
	      function fn2 () {
	         a++
	         console.log(a)
	      }
	      return fn2
	   }
	   var f = fn1()
	   
	   f()
	   f()
	   f = null
```
### 58.说说你对原型链的理解
```
1. 作用: 
	原型链用于查找对象的属性
2. 什么:
	实例对象上都会有一个隐式原型属性(__proto__), 它指向的就是原型对象, 而原型对象也有__proto__属性指向它的原型对象
3. 为什么__proto__指向的是原型对象?
   构造函数对象上有显式原型属性(prototype), 它指向的就是原型对象。
   实例对象的__proto__属性被赋值为构造函数的prototype属性值。
```
## 2019-03-23
### 59.在微信公众号开发中，客户端、微信服务器、开发者服务器三者如何交互？
![](https://i.loli.net/2019/03/22/5c94c92f19e22.png)

### 60.让一个DIV水平居中的几种方式
#### 第一种：
```css
#wrap{
  display: flex;
  justify-content: center;
  align-items: center;
}
```
#### 第二种：
```css
#wrap{
  position: absolute;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  margin: auto;
}
```
#### 第三种：
```css
#wrap{
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50% -50%);
}
```
### 61.H5有哪些新特性

1. 用于绘画 canvas 元素。
2. 用于媒体播放的 video 和 audio 元素。
3. 本地离线存储 localStorage 长期存储数据，浏览器关闭后数据不丢失；
   sessionStorage 的数据在浏览器关闭后自动删除。
4. 语意化更好的内容元素，比如 article、footer、header、nav、section
5. 表单控件，calendar、date、time、email、url、search。

### 62.请写一下深度复制（深度克隆）
```javascript
//深度克隆：对象/数组
function deepClone(target) {
    // 1. 初始化变量，获取目标数据的类型
    let result, targetClass = getTargetClass(target);
    // 2. 判断目标的类型
    if(targetClass === 'Object'){ // 对象
        result = {};
    }else if(targetClass === 'Array'){  // 数组
        result = [];
    }else {
        return target;
    }
    // 3. 遍历目标数据
    for(let key in target){
        // 获取每一项值
        let item = target[key];
        // 4. 判断每一项数据的类型
        if(getTargetClass(item) === 'Object' || 'Array'){
            // 无论是对象还是数组，都可以result[key]取值
            // item = {name: 'kobe'}  item[]
            result[key] = deepClone(item);
        }else {
            result[key] = item;
        }
    }
    return result;
}
```
## 2019-03-25
### 63.请说说什么是函数防抖、函数节流？什么时候使用防抖与节流？
#### 函数防抖：
1. 概念： 延迟要执行的动作，若在延时的这段时间内，再次触发，则取消之前开启的动作，重新计时。
2. 举例：电梯进人之后，延时5秒启动，这5秒钟之内如果在进来人，不计算之前的，重新计时5秒。
3. 实现： 定时器
4. 应用：搜索时等用户完整输入内容后再发送查询请求.
```javascript
  //获取要绑定事件的DOm
  var userInput = document.getElementById('user_input');
  //定义变量保存计时器id
  var timeid = null
  //给DOM绑定事件
  userInput.addEventListener('keyup',function () {
    var data = userInput.value;
    //清除上一次“动作”
    clearTimeout(timeid)
    //延迟要执行的“动作”
    timeid = setTimeout(function () {
      //具体“动作”
      Search(data)
    },300)
  })
```
#### 函数节流：
1. 概念：设定特定的时间，让函数在特定时间内置执行一次，不会频繁执行；
2. 举例：fps游戏，鼠标按住不松手，子弹也不会连成一条线；
3. 实现：标识、定时器；
4. 应用：禁止滚动条绑定的事件频繁触发
```javascript
	window.addEventListener('DOMContentLoaded',function () {
		  //标识是否可以触发动作
	      var flag = true 
		  //绑定事件
	      document.getElementsByTagName('body')[0].onscroll = function () {
	        if(flag){
	          console.log('scroll')
			  //标识为动作不可触发
	          flag = false;
	          setTimeout(function () {
	            flag = true;
	          },1000)
	        }
	      }
	   })
```
### 64.简单说一下css盒模型
	包含了元素内容（content）、内边距（padding）、边框（border）、外边距（margin）几个要素
![](http://www.w3school.com.cn/i/ct_boxmodel.gif)

### 65.说说微信公众号开发中access_token的作用，以及它的特点
```
1.是什么？
是公众号的全局唯一接口调用凭据。（不同用户的access_token是不一样的）
2.怎么用？
每次在调用微信接口的时候，都必须携带着access_token
3.特点：
 1> 有效时间是2小时，要求开发人员最好每隔一段时间去更新这个access_token，最好提前5分钟
 2> 获取access_token是有次数限制的，一般认证的账号是2000次/天，最好把获取到的access_token保存起来
 3> 包含着过期时间，说明access_token有效期是多久
 4> 2小时之内，如果再次请求新的，会导致上一次的失效（在即将过期的前5分钟去请求新的，会导致之前的，和新的access_token都可用
```
## 2019-03-26
### 66.简述一下git的pull与fetch的区别
```
pull: 拉去远程的更新到本地仓库的当前分支，并自动合并
fetch: 拉取远程的更新到本地仓库的新分支上
```
### 67.请编写一个数组去重的方法
```javascript
Array.prototype.unique=function() {
  var temp=[];
  for(var i=0;i<this.length;i++){
      if(temp.indexOf(this[i]==-1)
          temp.push(this[i])
    return temp;
  }
}
```
例如将[1,2,2,3,4,4,4,5,5]变为：[1,2,3,4,5]

### 68.请编写代码将一个字符串倒序，例如将'abcde'变为：'edcba'
```javascript
'abcde'.split('').reverse().join('');
```
### 69.哪些其他类型转Boolean时为false
```
undefined
null
false
NaN
''
0
-0
```
## 2019-03-27
### 70.说一下你对项目构建工具的了解
    1). 构建项目到底做些什么
       编译项目中的js, sass, less, stylus
       合并js/css等资源文件
       压缩js/css/html等资源文件
       JS语法的检查
    2). 构建工具
       作用: 简化项目构建, 实现自动化构建
       常用: webpack、gulp等
### 71.webpack配置文件的整体结构
```javascript
module.exports = {
     entry: '',	//入口
     output: {},	//出口
     module: {rules: []},	//loader
     plugins: []	//插件
   }
```
### 72.说一下webpack中的几个核心概念
    Entry：入口，Webpack进行打包的起始点(文件)
    Output：出口，webpack编译打包生成的bundle(文件)
    Loader：模块加载(转换)器，将非js模块包装成webpack能理解的js模块
    Plugin：插件，在 Webpack 构建流程中的特定时机插入具有特定功能的代码
    Module：模块，在 Webpack眼里一切皆模块，默认只识别js文件, 如果是其它类型文件利用对应的loader转换为js模块
### 73.比较一下webpack中的loader与plugin
    1. loader:  用于加载特定类型的资源文件, webpack本身只能打包js, 如果打包css就需要css-loader/style-loader, 如果打包图片就需要file-loader或者url-loader
    2. plugin: 用来扩展webpack其它方面的功能, 如生成页面需要html-webpack-plugin, 压缩js需要uglifyjs-webpack-plugin
## 2019-03-30 React
### 74. 说说你对React的基本理解
    1). 什么
       动态构建用户界面的JS库
    2). React的特点
       Declarative(声明式编码)
       Component-Based(组件化编码)
       高效
    3). React高效的原因
       虚拟(virtual)DOM, 不总是直接操作真实DOM(批量更新, 减少更新的次数) 
       高效的DOM Diff算法, 最小化DOM更新
### 75. 说说jSX	
    1). JSX 是一个看起来很像 XML 的 js 语法扩展
    2). 作用: 创建虚拟DOM(元素对象)
    2). 浏览器不能直接运行, 需要使用babel转换成纯JS语法: React.createElement()
    4). 相比于纯JS要编码更简洁易读
    5). 注意: JSX标签必须有结束, 组件标签首字母必须大写
### 76. 使用2种方式定义一个简单组件, 并区别它们
    定义
        1). 方式1: 工厂函数(简单/无状态组件)
           function MyComponent1(props) {
              return <h1>自定义组件标题11111</h1>
           }
        2). 方式2: ES6类(复杂/有状态组件)
           class MyComponent2 extends React.Component {
              render () { // 回调函数, 为组件对象提供虚拟DOM
                return <h1>自定义组件标题33333</h1>
              }
           }
    区别:
        1). 类组件: 使用class定义的组件, 会产生组件对象, 可以有自身的状态和生命周期勾子
        2). 函数组件: 使用function定义的组件, 不会产生组件对象, 没有自身的状态和生命周期勾子
### 77. 区别组件对象的3大属性
    1). state: 值为容器对象, 保存的是组件内可变的数据,组件根据state中的数据显示, 要更新界面只要更新state即可 
    2). props: 值为容器对象, 保存的是从组件外传递过来的数据, 当前组件只读, 父组件修改了自动显示新数据
    3). refs: 值为容器对象, 保存的是n个有ref属性的dom元素对象, 属性名是ref指定的标识名称, 值为对应的dom元素
### 78. React组件化编码的3步与2个重要问题
    编码的3步
       1). 拆分组件
       2). 实现静态组件
       3). 实现动态组件
          a. 动态显示初始化数据
             1. 类型
             2. 名称
             3. 保存在哪个组件
          b. 交互
    2个重要问题
       1). 状态数据保存在哪个组件?  看是某个组件需要, 还是某些组件需要(给父组件)
       2). 更新状态数据的行为在哪个组件? 状态在哪个组件, 行为就定义在哪个组件
### 79. 说说你对模块与模块化, 组件与组件化的理解
    1). 模块: 具有特定功能的js文件
    2). 模块化: 如果项目的JS编写是以多模块编写组合使用的, 那这个项目就是一个模块化的项目, 也就是模块化的编码方式
    2). 组件: 实现局部功能界面的所有代码的集合
    3). 组件化: 如果项目的功能界面是由多个组件组合编写实现的, 那这个项目就是一个组件化的项目, 也就是组件的编码方式

### 80. 调用 setState 之后发生了什么？
    1). React 会将传入的参数对象与组件当前的状态合并产生了新的state
    2). 生成新的虚拟DOM树  ==> render()
    3). 计算出新树与老树的节点差异，然后做真实DOM的差异更新
## 2019-04-01
day02
### 81. 说说受控组件与非受制组件
    受控组件: 包含Form输入的组件, 且在用户输入数据时自动实时收集到状态中, 相关技术: onChange + state
    非受控组件: 包含Form输入的组件, 用户输入的数据在提交表单请求才手动收集, 相关技术: ref
### 82. 说说组件的生命周期
    一、初始化阶段：ReactDOM.render(<Xxx/>, containDom)
       constructor(): 构造器方法
       componentWillMount：组件即将被挂载
       *render:生成/返回对应的虚拟DOM
       *componentDidMount:组件真正在被挂载(初始显示)之后
    二、更新阶段：this.setState({})
       *componentWillReceiveProps:组件将要接收到新的属性时调用
       *shouldComponentUpdate:组件接受到新属性或者新状态的时候调用（如果返回false,不调用render(), 否则调用）
       componentWillUpdate:组件将要更新
       *render:生成/返回新的虚拟DOM
       componentDidUpdate:组件已经更新
    三、销毁阶段：ReactDOM.unmountComponentAtNode(div)
       *componentWillUnmount:组件即将销毁
### 83. 说说react中的key的作用  | 为什么key不要使用index
    1. 虚拟DOM的key的作用?
       1). 简单说: key是虚拟DOM对象的标识, 在更新显示时key起着极其重要的作用
       2). 详细说: 当列表数组中的数据发生变化生成新的虚拟DOM后, React进行新旧虚拟DOM的diff比较
           a. key没有变
               item数据没变, 直接使用原来的真实DOM
               item数据变了, 对原来的真实DOM进行数据更新
           b. key变了
               销毁原来的真实DOM, 根据item数据创建新的真实DOM显示(即使item数据没有变)
    2. key为index的问题
       1). 添加/删除/排序 => 产生没有必要的真实DOM更新 ==> 界面效果没问题, 但效率低
       2). 如果item界面还有输入框 => 产生错误的真实DOM更新 ==> 界面有问题
       注意: 如果不存在添加/删除/排序操作, 用index没有问题
    3. 解决:
       使用item数据的标识数据作为key, 比如id属性值
### 84. 说说脚手架的理解
    1). 用来创建基于某个特定库(react/vue)的模板项目的工具包
    2). 全局下载脚手架后, 就会多出一个命令, 通过命令就可以创建项目
    3). 创建出的项目已经是有完整配置, 依赖声明的一个模块化/组件化/工程化的项目
### 85. 说说项目的开发环境运行与生产环境打包运行
    1). 开发环境运行
       a. 命令:
          npm start/ npm  run dev
       b. 背后做了什么
          在内存中生成打包文件(不生成本地打包文件)
          启动服务器运行内存中的打包文件
    2). 生产环境打包运行
       a. 命令
          npm run build
          serve build/dist
       b. 背后做了什么
          在内存中生成打包文件
          生成本地打包文件
          启动服务器加载运行本地打包文件
### 86. 为什么虚拟dom和dom diff算法能提高性能?
虚拟dom相当于在js和真实dom中间加了一个缓存，利用dom diff算法避免了没有必要的dom操作，从而提高性能。具体实现步骤如下：
```
 1). 用一般JS对象结构表示 DOM 树的结构, 然后用这个树构建一个真正的DOM树显示到页面
 2). 当状态更新的时候，重新构造一棵新的对象树
 3). 然后用新的树和旧的树进行diff比较, 得到虚拟DOM的差异, 并转化成对应的DOM更新
```
![](https://i.imgur.com/3byb09Z.png)
### 87. package.json的结构
```json
{
   "name": "react-demo", // 标识名称
   "version": "1.0.0", // 版本号
   "scripts": { // 打包运行相关的npm命令
      "xxx": "具体命令"   npm run xxx
   },
   dependencies: {}, // 运行时依赖
   devDependencies: {} // 开发时依赖
}
```
### 88. 区别React中的事件与原生的DOM事件
    1). 为了解决跨浏览器兼容性问题，React 会将浏览器原生事件封装为合成事件
    2). React 并没有直接将事件附着到子元素上, 而是将事件绑定在了组件的根元素上(使用事件委托)
### 89. 命令式编程与声明式编程
    声明式编程: 只关注做什么, 而不关注怎么做(流程),类似于填空题,声明式编程是建立命令式编程的基础上
    命令式编程: 要关注做什么和怎么做(流程), 类似于问答题
    数组中常见声明式方法: map() / forEach() / find() / findIndex()
```javascript
var arr = [1, 3, 5, 7]
// 需求: 得到一个新的数组, 数组中每个元素都比arr中对应的元素大10: [11, 13, 15, 17]
// 命令式编程
    var arr2 = []
    for(var i =0;i<arr.length;i++) {
      arr2.push(arr[i]+10)
    }
    console.log(arr2)
// 声明式编程
    var arr3 = arr.map(function(item){
      return item +10
    })
```
# 来自掘金
## 原生js
### 1. 基本类型有哪几种？null 是对象吗？基本数据类型和复杂数据类型存储有什么区别？
    * 基本类型有6种，分别是undefined,null,bool,string,number,symbol(ES6新增)。
    * 虽然 typeof null 返回的值是 object,但是null不是对象，而是基本数据类型的一种。
    * 基本数据类型存储在栈内存，存储的是值。
    * 复杂数据类型存储在堆内存，存储的是地址。当我们把对象赋值给另外一个变量的时候，复制的是地址，指向同一块内存空间，当其中一个对象改变时，另一个对象也会变化。

### 2.typeof 是否正确判断类型? instanceof呢？ instanceof 的实现原理是什么？
```javascript
首先 typeof 能够正确的判断基本数据类型，但是除了 null, typeof null输出的是对象。
但是对象来说，typeof 不能正确的判断其类型， typeof 一个函数可以输出 'function',而除此之外，输出的全是 object,这种情况下，我们无法准确的知道对象的类型。
instanceof可以准确的判断复杂数据类型，但是不能正确判断基本数据类型。(正确判断数据类型请戳：github.com/YvetteLau/B…)
instanceof 是通过原型链判断的，A instanceof B, 在A的原型链中层层查找，是否有原型等于B.prototype，如果一直找到A的原型链的顶端(null;即Object.prototype.__proto__),仍然不等于B.prototype，那么返回false，否则返回true.
instanceof的实现代码:
// L instanceof R
function instance_of(L, R) {//L 表示左表达式，R 表示右表达式
    var O = R.prototype;// 取 R 的显式原型
    L = L.__proto__;    // 取 L 的隐式原型
    while (true) { 
        if (L === null) //已经找到顶层
            return false;  
        if (O === L)   //当 O 严格等于 L 时，返回 true
            return true; 
        L = L.__proto__;  //继续向上一层原型链查找
    } 
}
```
### 3.for of , for in 和 forEach,map 的区别。

```javascript
for...of循环：具有 iterator 接口，就可以用for...of循环遍历它的成员(属性值)。for...of循环可以使用的范围包括数组、Set 和 Map 结构、某些类似数组的对象、Generator 对象，以及字符串。for...of循环调用遍历器接口，数组的遍历器接口只返回具有数字索引的属性。对于普通的对象，for...of结构不能直接使用，会报错，必须部署了 Iterator 接口后才能使用。可以中断循环。
for...in循环：遍历对象自身的和继承的可枚举的属性, 不能直接获取属性值。可以中断循环。
forEach: 只能遍历数组，不能中断，没有返回值(或认为返回值是undefined)。
map: 只能遍历数组，不能中断，返回值是修改后的数组。

PS: Object.keys()：返回给定对象所有可枚举属性的字符串数组。
关于forEach是否会改变原数组的问题，有些小伙伴提出了异议，为此我写了代码测试了下(注意数组项是复杂数据类型的情况)。
除了forEach之外，map等API，也有同样的问题。
let arry = [1, 2, 3, 4];
arry.forEach((item) => {
    item *= 10;
});
console.log(arry); //[1, 2, 3, 4]
arry.forEach((item) => {
    arry[1] = 10; //直接操作数组
});
console.log(arry); //[ 1, 10, 3, 4 ]
let arry2 = [
    { name: "Yve" },
    { age: 20 }
];
arry2.forEach((item) => {
    item.name = 10;
});
console.log(arry2);//[ { name: 10 }, { age: 20, name: 10 } ]
```
### 4.如何判断一个变量是不是数组？
```javascript
使用 Array.isArray 判断，如果返回 true, 说明是数组
使用 instanceof Array 判断，如果返回true, 说明是数组
使用 Object.prototype.toString.call 判断，如果值是 [object Array], 说明是数组
通过 constructor 来判断，如果是数组，那么 arr.constructor === Array. (不准确，因为我们可以指定 obj.constructor = Array)
function fn() {
    console.log(Array.isArray(arguments));   //false; 因为arguments是类数组，但不是数组
    console.log(Array.isArray([1,2,3,4]));   //true
    console.log(arguments instanceof Array); //fasle
    console.log([1,2,3,4] instanceof Array); //true
    console.log(Object.prototype.toString.call(arguments)); //[object Arguments]
    console.log(Object.prototype.toString.call([1,2,3,4])); //[object Array]
    console.log(arguments.constructor === Array); //false
    arguments.constructor = Array;
    console.log(arguments.constructor === Array); //true
    console.log(Array.isArray(arguments));        //false
}
fn(1,2,3,4);
```
### 5. 类数组和数组的区别是什么？
```javascript
类数组:
1）拥有length属性，其它属性（索引）为非负整数（对象中的索引会被当做字符串来处理）;
2）不具有数组所具有的方法；
类数组是一个普通对象，而真实的数组是Array类型。
常见的类数组有: 函数的参数 arugments, DOM 对象列表(比如通过 document.querySelectorAll 得到的列表), jQuery 对象 (比如 $("div")).
类数组可以转换为数组:
//第一种方法
Array.prototype.slice.call(arrayLike, start);
//第二种方法
[...arrayLike];
//第三种方法:
Array.from(arrayLike);
PS: 任何定义了遍历器（Iterator）接口的对象，都可以用扩展运算符转为真正的数组。
Array.from方法用于将两类对象转为真正的数组：类似数组的对象（array-like object）和可遍历（iterable）的对象。
```
### 6.== 和 === 有什么区别？
```javascript
  === 不需要进行类型转换，只有类型相同并且值相等时，才返回 true.
  == 如果两者类型不同，首先需要进行类型转换。具体流程如下:
  首先判断两者类型是否相同，如果相等，判断值是否相等.
  如果类型不同，进行类型转换
  判断比较的是否是 null 或者是 undefined, 如果是, 返回 true .
  判断两者类型是否为 string 和 number, 如果是, 将字符串转换成 number
  判断其中一方是否为 boolean, 如果是, 将 boolean 转为 number 再进行判断
  判断其中一方是否为 object 且另一方为 string、number 或者 symbol , 如果是, 将 object 转为原始类型再进行判断
  let person1 = {
      age: 25
  }
  let person2 = person1;
  person2.gae = 20;
  console.log(person1 === person2); //true,注意复杂数据类型，比较的是引用地址
  思考: [] == ![]
  我们来分析一下: [] == ![] 是true还是false？
  首先，我们需要知道 ! 优先级是高于 == (更多运算符优先级可查看: 运算符优先级)
  ![] 引用类型转换成布尔值都是true,因此![]的是false
  根据上面的比较步骤中的第五条，其中一方是 boolean，将 boolean 转为 number 再进行判断，false转换成 number，对应的值是 0.
  根据上面比较步骤中的第六条，有一方是 number，那么将object也转换成Number,空数组转换成数字，对应的值是0.(空数组转换成数字，对应的值是0，如果数组中只有一个数字，那么转成number就是这个数字，其它情况，均为NaN)
  0 == 0; 为true
```
### 7. ES6中的class和ES5的类有什么区别？
    ES6 class 内部所有定义的方法都是不可枚举的;
    ES6 class 必须使用 new 调用;
    ES6 class 不存在变量提升;
    ES6 class 默认即是严格模式;
    ES6 class 子类必须在父类的构造函数中调用super()，这样才有this对象;ES5中类继承的关系是相反的，先有子类的this，然后用父类的方法应用在this上。
### 数组的哪些API会改变原数组？
    修改原数组的API有: splice/reverse/fill/copyWithin/sort/push/pop/unshift/shift
    不修改原数组的API有:slice/map/forEach/every/filter/reduce/entries/find
    注: 数组的每一项是简单数据类型，且未直接操作数组的情况下(稍后会对此题重新作答)。
### 9. let、const 以及 var 的区别是什么？
    let 和 const 定义的变量不会出现变量提升，而 var 定义的变量会提升。
    let 和 const 是JS中的块级作用域
    let 和 const 不允许重复声明(会抛出错误)
    let 和 const 定义的变量在定义语句之前，如果使用会抛出错误(形成了暂时性死区)，而 var 不会。
    const 声明一个只读的常量。一旦声明，常量的值就不能改变(如果声明是一个对象，那么不能改变的是对象的引用地址)
### 10. 在JS中什么是变量提升？什么是暂时性死区？
    变量提升就是变量在声明之前就可以使用，值为undefined。
    在代码块内，使用 let/const 命令声明变量之前，该变量都是不可用的(会抛出错误)。这在语法上，称为“暂时性死区”。暂时性死区也意味着 typeof 不再是一个百分百安全的操作。
    typeof x; // ReferenceError(暂时性死区，抛错)
    let x;
    typeof y; // 值是undefined,不会报错
    暂时性死区的本质就是，只要一进入当前作用域，所要使用的变量就已经存在了，但是不可获取，只有等到声明变量的那一行代码出现，才可以获取和使用该变量。
### 11. 如何正确的判断this? 箭头函数的this是什么？
```javascript
this的绑定规则有四种：默认绑定，隐式绑定，显式绑定，new绑定.

函数是否在 new 中调用(new绑定)，如果是，那么 this 绑定的是新创建的对象。
函数是否通过 call,apply 调用，或者使用了 bind (即硬绑定)，如果是，那么this绑定的就是指定的对象。
函数是否在某个上下文对象中调用(隐式绑定)，如果是的话，this 绑定的是那个上下文对象。一般是 obj.foo()
如果以上都不是，那么使用默认绑定。如果在严格模式下，则绑定到 undefined，否则绑定到全局对象。
如果把 null 或者 undefined 作为 this 的绑定对象传入 call、apply 或者 bind, 这些值在调用时会被忽略，实际应用的是默认绑定规则。
箭头函数没有自己的 this, 它的this继承于上一层代码块的this。

测试下是否已经成功Get了此知识点(浏览器执行环境):
var number = 5;
var obj = {
    number: 3,
    fn1: (function () {
        var number;
        this.number *= 2;
        number = number * 2;
        number = 3;
        return function () {
            var num = this.number;
            this.number *= 2;
            console.log(num);
            number *= 3;
            console.log(number);
        }
    })()
}
var fn1 = obj.fn1;
fn1.call(null);
obj.fn1();
console.log(window.number);
```
### 12. 词法作用域和this的区别。
    词法作用域是由你在写代码时将变量和块作用域写在哪里来决定的
    this 是在调用时被绑定的，this 指向什么，完全取决于函数的调用位置(关于this的指向问题，本文已经有说明)
### 13. 谈谈你对JS执行上下文栈和作用域链的理解。
    执行上下文就是当前 JavaScript 代码被解析和执行时所在环境, JS执行上下文栈可以认为是一个存储函数调用的栈结构，遵循先进后出的原则。
    JavaScript执行在单线程上，所有的代码都是排队执行。
    一开始浏览器执行全局的代码时，首先创建全局的执行上下文，压入执行栈的顶部。
    每当进入一个函数的执行就会创建函数的执行上下文，并且把它压入执行栈的顶部。当前函数执行-完成后，当前函数的执行上下文出栈，并等待垃圾回收。
    浏览器的JS执行引擎总是访问栈顶的执行上下文。
    全局上下文只有唯一的一个，它在浏览器关闭时出栈。
    作用域链: 无论是 LHS 还是 RHS 查询，都会在当前的作用域开始查找，如果没有找到，就会向上级作用域继续查找目标标识符，每次上升一个作用域，一直到全局作用域为止。
## JS高级
### 14. 什么是闭包？闭包的作用是什么？闭包有哪些使用场景？
    闭包是指有权访问另一个函数作用域中的变量的函数，创建闭包最常用的方式就是在一个函数内部创建另一个函数。
    闭包的作用有:
      封装私有变量
      模仿块级作用域(ES5中没有块级作用域)
      实现JS的模块
### 15. call、apply有什么区别？call,aplly和bind的内部是如何实现的？
```javascript
call 和 apply 的功能相同，区别在于传参的方式不一样:
fn.call(obj, arg1, arg2, ...),调用一个函数, 具有一个指定的this值和分别地提供的参数(参数的列表)。
fn.apply(obj, [argsArray]),调用一个函数，具有一个指定的this值，以及作为一个数组（或类数组对象）提供的参数。
call核心:
  将函数设为传入参数的属性
  指定this到函数并传入给定参数执行函数
  如果不传入参数或者参数为null，默认指向为 window / global
  删除参数上的函数

Function.prototype.call = function (context) {
    /** 如果第一个参数传入的是 null 或者是 undefined, 那么指向this指向 window/global */
    /** 如果第一个参数传入的不是null或者是undefined, 那么必须是一个对象 */
    if (!context) {
        //context为null或者是undefined
        context = typeof window === 'undefined' ? global : window;
    }
    context.fn = this; //this指向的是当前的函数(Function的实例)
    let rest = [...arguments].slice(1);//获取除了this指向对象以外的参数, 空数组slice后返回的仍然是空数组
    let result = context.fn(...rest); //隐式绑定,当前函数的this指向了context.
    delete context.fn;
    return result;
}
//测试代码
var foo = {
    name: 'Selina'
}
var name = 'Chirs';
function bar(job, age) {
    console.log(this.name);
    console.log(job, age);
}
bar.call(foo, 'programmer', 20);
// Selina programmer 20
bar.call(null, 'teacher', 25);
// 浏览器环境: Chirs teacher 25; node 环境: undefined teacher 25
==================================
apply:
apply的实现和call很类似，但是需要注意他们的参数是不一样的，apply的第二个参数是数组或类数组.
Function.prototype.apply = function (context, rest) {
    if (!context) {
        //context为null或者是undefined时,设置默认值
        context = typeof window === 'undefined' ? global : window;
    }
    context.fn = this;
    let result;
    if(rest === undefined || rest === null) {
        //undefined 或者 是 null 不是 Iterator 对象，不能被 ...
        result = context.fn(rest);
    }else if(typeof rest === 'object') {
        result = context.fn(...rest);
    }
    delete context.fn;
    return result;
}
var foo = {
    name: 'Selina'
}
var name = 'Chirs';
function bar(job, age) {
    console.log(this.name);
    console.log(job, age);
}
bar.apply(foo, ['programmer', 20]);
// Selina programmer 20
bar.apply(null, ['teacher', 25]);
// 浏览器环境: Chirs programmer 20; node 环境: undefined teacher 25
============================================
bind
bind 和 call/apply 有一个很重要的区别，一个函数被 call/apply 的时候，会直接调用，但是 bind 会创建一个新函数。当这个新函数被调用时，bind() 的第一个参数将作为它运行时的 this，之后的一序列参数将会在传递的实参前传入作为它的参数。
Function.prototype.bind = function(context) {
    if(typeof this !== "function"){
       throw new TypeError("not a function");
    }
    let self = this;
    let args = [...arguments].slice(1);
    function Fn() {};
    Fn.prototype = this.prototype;
    let bound = function() {
        let res = [...args, ...arguments]; //bind传递的参数和函数调用时传递的参数拼接
        context = this instanceof Fn ? this : context || this;
        return self.apply(context, res);
    }
    //原型链
    bound.prototype = new Fn();
    return bound;
}
var name = 'Jack';
function person(age, job, gender){
    console.log(this.name , age, job, gender);
}
var Yve = {name : 'Yvette'};
let result = person.bind(Yve, 22, 'enginner')('female');	
```
### 16. new的原理是什么？通过new的方式创建对象和通过字面量创建有什么区别？
    new:
    创建一个新对象。
    这个新对象会被执行[[原型]]连接。
    将构造函数的作用域赋值给新对象，即this指向这个新对象.
    如果函数没有返回其他对象，那么new表达式中的函数调用会自动返回这个新对象。
    function new(func) {
        let target = {};
        target.__proto__ = func.prototype;
        let res = func.call(target);
        if (typeof(res) == "object" || typeof(res) == "function") {
        	return res;
        }
        return target;
    }
    字面量创建对象，不会调用 Object构造函数, 简洁且性能更好;
    new Object() 方式创建对象本质上是方法调用，涉及到在proto链中遍历该方法，当找到该方法后，又会生产方法调用必须的 堆栈信息，方法调用结束后，还要释放该堆栈，性能不如字面量的方式。
    通过对象字面量定义对象时，不会调用Object构造函数
### 17. 谈谈你对原型的理解？
    在 JavaScript 中，每当定义一个对象（函数也是对象）时候，对象中都会包含一些预定义的属性。其中每个函数对象都有一个prototype 属性，这个属性指向函数的原型对象。使用原型对象的好处是所有对象实例共享它所包含的属性和方法。
### 18. 什么是原型链？【原型链解决的是什么问题？】
    原型链解决的主要是继承问题。
    每个对象拥有一个原型对象，通过 proto (读音: dunder proto) 指针指向其原型对象，并从中继承方法和属性，同时原型对象也可能拥有原型，这样一层一层，最终指向 null(Object.proptotype.__proto__ 指向的是null)。这种关系被称为原型链 (prototype chain)，通过原型链一个对象可以拥有定义在其他对象中的属性和方法。
    构造函数 Parent、Parent.prototype 和 实例 p 的关系如下:(p.__proto__ === Parent.prototype)
![关系图](https://user-gold-cdn.xitu.io/2019/4/8/169fc27e0cf46ee9?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)
### 19. prototype 和 __proto__ 区别是什么？
```javascript
prototype是构造函数的属性。
__proto__ 是每个实例都有的属性，可以访问 [[prototype]] 属性。
实例的__proto__ 与其构造函数的prototype指向的是同一个对象。
function Student(name) {
    this.name = name;
}
Student.prototype.setAge = function(){
    this.age=20;
}
let Jack = new Student('jack');
console.log(Jack.__proto__);
//console.log(Object.getPrototypeOf(Jack));;
console.log(Student.prototype);
console.log(Jack.__proto__ === Student.prototype);//true
```
### 20. 使用ES5实现一个继承？
    组合继承(最常用的继承方式)
    function SuperType() {
        this.name = name;
        this.colors = ['red', 'blue', 'green'];
    }
    SuperType.prototype.sayName = function() {
        console.log(this.name);
    }
    
    function SubType(name, age) {
        SuperType.call(this, name);
        this.age = age;
    }
    SubType.prototype = new SuperType();
    SubType.prototype.constructor = SubType;
    
    SubType.prototype.sayAge = function() {
        console.log(this.age);
    }
    其它继承方式实现，可以参考《JavaScript高级程序设计》
### 21. 什么是深拷贝？深拷贝和浅拷贝有什么区别？
    浅拷贝是指只复制第一层对象，但是当对象的属性是引用类型时，实质复制的是其引用，当引用指向的值改变时也会跟着变化。
    深拷贝复制变量值，对于非基本类型的变量，则递归至基本类型变量后，再复制。深拷贝后的对象与原来的对象是完全隔离的，互不影响，对一个对象的修改并不会影响另一个对象。
    实现一个深拷贝:
    function deepClone(obj) { //递归拷贝
        if(obj === null) return null; //null 的情况
        if(obj instanceof RegExp) return new RegExp(obj);
        if(obj instanceof Date) return new Date(obj);
        if(typeof obj !== 'object') {
            //如果不是复杂数据类型，直接返回
            return obj;
        }
        /**
         * 如果obj是数组，那么 obj.constructor 是 [Function: Array]
         * 如果obj是对象，那么 obj.constructor 是 [Function: Object]
         */
        let t = new obj.constructor();
        for(let key in obj) {
            //如果 obj[key] 是复杂数据类型，递归
            t[key] = deepClone(obj[key]);
        }
        return t;
    }
### 22. 防抖和节流的区别是什么？防抖和节流的实现。
```javascript
防抖和节流的作用都是防止函数多次调用。区别在于，假设一个用户一直触发这个函数，且每次触发函数的间隔小于设置的时间，防抖的情况下只会调用一次，而节流的情况会每隔一定时间调用一次函数。
防抖(debounce): n秒内函数只会执行一次，如果n秒内高频事件再次被触发，则重新计算时间
function debounce(func, wait, immediate=true) {
    let timeout, context, args;
        // 延迟执行函数
        const later = () => setTimeout(() => {
            // 延迟函数执行完毕，清空定时器
            timeout = null
            // 延迟执行的情况下，函数会在延迟函数中执行
            // 使用到之前缓存的参数和上下文
            if (!immediate) {
                func.apply(context, args);
                context = args = null;
            }
        }, wait);
        let debounced = function (...params) {
            if (!timeout) {
                timeout = later();
                if (immediate) {
                    //立即执行
                    func.apply(this, params);
                } else {
                    //闭包
                    context = this;
                    args = params;
                }
            } else {
                clearTimeout(timeout);
                timeout = later();
            }
        }
    debounced.cancel = function () {
        clearTimeout(timeout);
        timeout = null;
    };
    return debounced;
};
防抖的应用场景:
  每次 resize/scroll 触发统计事件
  文本输入的验证（连续输入文字后发送 AJAX 请求进行验证，验证一次就好）
===============================================================
节流(throttle): 高频事件在规定时间内只会执行一次，执行一次后，只有大于设定的执行周期后才会执行第二次。
//underscore.js
function throttle(func, wait, options) {
    var timeout, context, args, result;
    var previous = 0;
    if (!options) options = {};

    var later = function () {
        previous = options.leading === false ? 0 : Date.now() || new Date().getTime();
        timeout = null;
        result = func.apply(context, args);
        if (!timeout) context = args = null;
    };
    var throttled = function () {
        var now = Date.now() || new Date().getTime();
        if (!previous && options.leading === false) previous = now;
        var remaining = wait - (now - previous);
        context = this;
        args = arguments;
        if (remaining <= 0 || remaining > wait) {
            if (timeout) {
                clearTimeout(timeout);
                timeout = null;
            }
            previous = now;
            result = func.apply(context, args);
            if (!timeout) context = args = null;
        } else if (!timeout && options.trailing !== false) {
            // 判断是否设置了定时器和 trailing
            timeout = setTimeout(later, remaining);
        }
        return result;
    };
    throttled.cancel = function () {
        clearTimeout(timeout);
        previous = 0;
        timeout = context = args = null;
    };
    return throttled;
};

函数节流的应用场景有:
  DOM 元素的拖拽功能实现（mousemove）
  射击游戏的 mousedown/keydown 事件（单位时间只能发射一颗子弹）
  计算鼠标移动的距离（mousemove）
  Canvas 模拟画板功能（mousemove）
  搜索联想（keyup）
  监听滚动事件判断是否到页面底部自动加载更多：给 scroll 加了 debounce 后，只有用户停止滚动后，才会判断是否到了页面底部；如果是 throttle 的话，只要页面滚动就会间隔一段时间判断一次
```
### 23. 取数组的最大值（ES5、ES6）
```javascript
// ES5 的写法
Math.max.apply(null, [14, 3, 77, 30]);
// ES6 的写法
Math.max(...[14, 3, 77, 30]);
// reduce
[14,3,77,30].reduce((accumulator, currentValue)=>{
    return accumulator = accumulator > currentValue ? accumulator : currentValue
});
```
### 24. ES6新的特性有哪些？
    * 新增了块级作用域(let,const)
    * 提供了定义类的语法糖(class)
    * 新增了一种基本数据类型(Symbol)
    * 新增了变量的解构赋值
    * 函数参数允许设置默认值，引入了rest参数，新增了箭头函数
    * 数组新增了一些API，如 isArray / from / of 方法;数组实例新增了 entries()，keys() 和 values() 等方法
    * 对象和数组新增了扩展运算符
    * ES6 新增了模块化(import/export)
    * ES6 新增了 Set 和 Map 数据结构
    * ES6 原生提供 Proxy 构造函数，用来生成 Proxy 实例
    * ES6 新增了生成器(Generator)和遍历器(Iterator)
### 25. setTimeout倒计时为什么会出现误差？
    setTimeout() 只是将事件插入了“任务队列”，必须等当前代码（执行栈）执行完，主线程才会去执行它指定的回调函数。要是当前代码消耗时间很长，也有可能要等很久，所以并没办法保证回调函数一定会在 setTimeout() 指定的时间执行。所以， setTimeout() 的第二个参数表示的是最少时间，并非是确切时间。
    HTML5标准规定了 setTimeout() 的第二个参数的最小值不得小于4毫秒，如果低于这个值，则默认是4毫秒。在此之前。老版本的浏览器都将最短时间设为10毫秒。另外，对于那些DOM的变动（尤其是涉及页面重新渲染的部分），通常是间隔16毫秒执行。这时使用 requestAnimationFrame() 的效果要好于 setTimeout();
### 26. 为什么 0.1 + 0.2 != 0.3 ?
    0.1 + 0.2 != 0.3 是因为在进制转换和进阶运算的过程中出现精度损失。
    下面是详细解释:
    JavaScript使用 Number 类型表示数字(整数和浮点数)，使用64位表示一个数字。
![explain](https://user-gold-cdn.xitu.io/2019/4/8/169fc27e0dbdb30f?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)    
    图片说明:
      第0位：符号位，0表示正数，1表示负数(s)
      第1位到第11位：储存指数部分（e）
      第12位到第63位：储存小数部分（即有效数字）f
    计算机无法直接对十进制的数字进行运算, 需要先对照 IEEE 754 规范转换成二进制，然后对阶运算。
    1.进制转换
    0.1和0.2转换成二进制后会无限循环
    0.1 -> 0.0001100110011001...(无限循环)
    0.2 -> 0.0011001100110011...(无限循环)
    但是由于IEEE 754尾数位数限制，需要将后面多余的位截掉，这样在进制之间的转换中精度已经损失。
    2.对阶运算
    由于指数位数不相同，运算时需要对阶运算 这部分也可能产生精度损失。
    按照上面两步运算（包括两步的精度损失），最后的结果是
    0.0100110011001100110011001100110011001100110011001100
    结果转换成十进制之后就是 0.30000000000000004。
### 27.promise 有几种状态, Promise 有什么优缺点 ?
    promise有三种状态: fulfilled, rejected, pending.
    Promise 的优点:
      一旦状态改变，就不会再变，任何时候都可以得到这个结果
      可以将异步操作以同步操作的流程表达出来，避免了层层嵌套的回调函数
    Promise 的缺点:
      无法取消 Promise
      当处于pending状态时，无法得知目前进展到哪一个阶段
### 28. Promise构造函数是同步还是异步执行，then中的方法呢 ?promise如何实现then处理 ?
    Promise的构造函数是同步执行的。then 中的方法是异步执行的。
    promise的then实现
### 29. Promise和setTimeout的区别 ?
    Promise 是微任务，setTimeout 是宏任务，同一个事件循环中，promise.then总是先于 setTimeout 执行。
### 30. 如何实现 Promise.all ?
```javascript
要实现 Promise.all,首先我们需要知道 Promise.all 的功能：

如果传入的参数是一个空的可迭代对象，那么此promise对象回调完成(resolve),只有此情况，是同步执行的，其它都是异步返回的。
如果传入的参数不包含任何 promise，则返回一个异步完成.
promises 中所有的promise都“完成”时或参数中不包含 promise 时回调完成。
如果参数中有一个promise失败，那么Promise.all返回的promise对象失败
在任何情况下，Promise.all 返回的 promise 的完成状态的结果都是一个数组

Promise.all = function (promises) {
    return new Promise((resolve, reject) => {
        let index = 0;
        let result = [];
        if (promises.length === 0) {
            resolve(result);
        } else {
            function processValue(i, data) {
                result[i] = data;
                if (++index === promises.length) {
                    resolve(result);
                }
            }
            for (let i = 0; i < promises.length; i++) {
                //promises[i] 可能是普通值
                Promise.resolve(promises[i]).then((data) => {
                    processValue(i, data);
                }, (err) => {
                    reject(err);
                    return;
                });
            }
        }
    });
}
```
### 31.如何实现 Promise.finally ?
```javascript
不管成功还是失败，都会走到finally中,并且finally之后，还可以继续then。并且会将值原封不动的传递给后面的then.
Promise.prototype.finally = function (callback) {
    return this.then((value) => {
        return Promise.resolve(callback()).then(() => {
            return value;
        });
    }, (err) => {
        return Promise.resolve(callback()).then(() => {
            throw err;
        });
    });
}
```
### 32. 什么是函数柯里化？实现 sum(1)(2)(3) 返回结果是1,2,3之和
```javascript
函数柯里化是把接受多个参数的函数变换成接受一个单一参数（最初函数的第一个参数）的函数，并且返回接受余下的参数而且返回结果的新函数的技术。
function sum(a) {
    return function(b) {
        return function(c) {
            return a+b+c;
        }
    }
}
console.log(sum(1)(2)(3)); // 6
引申：实现一个curry函数，将普通函数进行柯里化:
function curry(fn, args = []) {
    return function(){
        let rest = [...args, ...arguments];
        if (rest.length < fn.length) {
            return curry.call(this,fn,rest);
        }else{
            return fn.apply(this,rest);
        }
    }
}
//test
function sum(a,b,c) {
    return a+b+c;
}
let sumFn = curry(sum);
console.log(sumFn(1)(2)(3)); //6
console.log(sumFn(1)(2, 3)); //6
```



