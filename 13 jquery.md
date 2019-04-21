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
