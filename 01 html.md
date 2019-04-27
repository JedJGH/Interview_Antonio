## HTML
### 1.H5新特性有哪些？
    * 语义化标签header,nav,footer,aside,article,section
    * 音频、视频API(audio,video)
    * 画布(Canvas) API
    * 地理(Geolocation) API
    * localStorage和sessionStorage
    * webworker, websocket
    * web worker是运行在浏览器后台的js程序，他不影响主程序的运行，是另开的一个js线程，可以用这个线程执行复杂的数据操作，然后把操作结果通过postMessage传递给主线程，这样在进行复杂且耗时的操作时就不会阻塞主线程了。
### 2.两种实现前端路由的方式
    ① <code>history.pushState</code> 
      
    ② <code>history.replaceState</code>
      
    DOM window 对象通过 history 对象提供了对浏览器的会话历史的访问，
    允许你在用户浏览历史中向前和向后跳转，同时——从HTML5开始——提供了对history栈中内容的操作
    window.history.back(); //在history中向后跳转(如同用户点击了后退 ← 按钮)
    window.history.forward(); //在history中向前跳转(如同用户点击了前进 → 按钮)
    window.history.go(N);跳转到 history 中指定的一个点
      window.history.go(-1) -- 向后移动一个页面 (等同于调用 back())
      window.history.go(1) -- 向前移动一个页面, 等同于调用了 forward():
### 前端路由的优、缺点:
    优点：
      * 比如页面持久性，像大部分音乐网站，你都可以在播放歌曲的同时，跳转到别的页面而音乐没有中断，再比如前后端彻底分离。
      * 开发一个前端路由，主要考虑到页面的可插拔、页面的生命周期、内存管理等。
      * 从性能和用户体验的层面来比较的话，后端路由每次访问一个新页面的时候都要向服务器发送请求，然后服务器再响应请求，这个过程肯定会有延迟。而前端路由在访问一个新页面的时候仅仅是变换了一下路径而已，没有了网络延迟，对于用户体验来说会有相当大的提升。
    缺点：使用浏览器的前进，后退键的时候会重新发送请求，没有合理地利用缓存。
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
