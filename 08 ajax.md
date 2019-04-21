## Ajax
### 1.Ajax
#### 1).ajax请求和原理
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
#### 2).AJAX的工作原理
    Ajax的工作原理相当于在用户和服务器之间加了—个中间层(AJAX引擎),使用户操作与服务器响应异步化。　Ajax的原理简单来说通过XmlHttpRequest对象来向服务器发异步请求，从服务器获得数据，然后用javascript来操作DOM而更新页面。
#### 3).ajax优缺点
    优点：无刷新更新数据 异步与服务器通信 前后端负载均衡
#### 4).缺点：
    ajax干掉了Back和history功能，对浏览器机制的破坏 2）对搜索引擎支持较弱 3）违背了URI和资源定位的初衷
#### 5).fetch和Ajax有什么不同
    fetch 是浏览器提供的一个新的 web API，它用来代替 Ajax（XMLHttpRequest），其提供了更优雅的接口，更灵活强大的功能。 Fetch 优点主要有：
        语法简洁，更加语义化
        基于标准 Promise 实现，支持 async/await
    
    fetch(url).then(response => response.json())
      .then(data => console.log(data))
      .catch(e => console.log("Oops, error", e))
    XMLHttpRequest 是一个设计粗糙的 API，不符合关注分离（Separation of Concerns）的原则，配置和调用方式非常混乱，而且基于事件的异步模型写起来也没有现代的 Promise，generator/yield，async/await 友好。
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
### jsonp缺点，为什么不能用POST(美团)
### cookie和localStorage区别(美团)
### 跨域(饿了么)
### xss和csrf(美团)
### 5. 获取当前网址url并从中截取信息。糯米
### 11. jsonp跨域问题，并手写代码 糯米
