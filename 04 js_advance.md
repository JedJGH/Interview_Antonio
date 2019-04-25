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
### 闭包
    特性： 1.函数嵌套函数 2.函数内部可以引用外部的参数和变量 3.参数和变量不会被垃圾回收机制回收
    闭包的缺点就是常驻内存，会增大内存使用量，使用不当很容易造成内存泄露。
    为什么要使用闭包：
    为了设计私有方法和变量，避免全局变量污染 希望一个变量长期驻扎在内存中
    view detail: https://segmentfault.com/a/1190000000652891
### 原型与原型链
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
### 作用域与作用域链
    作用域链的作用是保证执行环境里有权访问的变量和函数是有序的，作用域链的变量只能向上访问，变量访问到window对象即被终止，作用域链向下访问变量是不被允许的。
    全局函数无法查看局部函数的内部细节，但局部函数可以查看其上层的函数细节，直至全局细节。
    当需要从局部函数查找某一属性或方法时，如果当前作用域没有找到，就会上溯到上层作用域查找，
    直至全局函数，这种组织形式就是作用域链。
### apply, call和bind有什么区别?
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
### 谈谈对this的理解
    this总是指向函数的直接调用者（而非间接调用者）；
    如果有new关键字，this指向new出来的那个对象；
    在事件中，this指向触发这个事件的对象，特殊的是，IE中的attachEvent中的this总是指向全局对象Window；
### 那些操作会造成内存泄漏？
    内存泄漏指任何对象在您不再拥有或需要它之后仍然存在。
    垃圾回收器定期扫描对象，并计算引用了每个对象的其他对象的数量。如果一个对象的引用数量为 0（没有其他对象引用过该对象），或对该对象的惟一引用是循环的，那么该对象的内存即可回收。
    setTimeout 的第一个参数使用字符串而非函数的话，会引发内存泄漏。
    闭包、控制台日志、循环（在两个对象彼此引用且彼此保留时，就会产生一个循环）
### 深入贯彻闭包思想，全面理解JS闭包形成过程
    https://segmentfault.com/a/1190000009886713
### 下面这个ul，如何点击每一列的时候alert其index?（闭包）
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
### js继承方式及其优缺点
    原型链继承的缺点
    一 是字面量重写原型会中断关系，使用引用类型的原型，并且子类型还无法给超类型传递参数。
    借用构造函数（类式继承）
    借用构造函数虽然解决了刚才两种问题，但没有原型，则复用无从谈起。所以我们需要原型链+借用构造函数的模式，这种模式称为组合继承
    
    组合式继承
    组合式继承是比较常用的一种继承方法，其背后的思路是 使用原型链实现对原型属性和方法的继承，而通过借用构造函数来实现对实例属性的继承。这样，既通过在原型上定义方法实现了函数复用，又保证每个实例都有它自己的属性。
### 什么是上下文环境对象
1
### new操作符具体做了什么
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
### 创建对象的几种方式
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
### 什么是线程机制
    1.同步和异步的区别?
    同步：阻塞的
      -张三叫李四去吃饭，李四一直忙得不停，张三一直等着，直到李四忙完两个人一块去吃饭
      =浏览器向服务器请求数据，服务器比较忙，浏览器一直等着（页面白屏），直到服务器返回数据，浏览器才能显示页面
    异步：非阻塞的
      -张三叫李四去吃饭，李四在忙，张三说了一声然后自己就去吃饭了，李四忙完后自己去吃
      =浏览器向服务器请求数据，服务器比较忙，浏览器可以自如的干原来的事情（显示页面），服务器返回数据的时候通知浏览器一声，浏览器把返回的数据再渲染到页面，局部更新
### bind返回什么(饿了么)
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
### 事件委托原理(糯米)
1
### 事件代理和冒泡，捕获(美团)
1
### 事件捕获的应用(美团)
1
### 性能优化(美团)
1
### 闭包原理并应用 糯米
1
### 原型继承的几种方式 糯米
1
### 函数的2个角色, 方法与属性, 方法与函数
    1). 函数的2个角色:
        函数: 通过()调用
        对象: 通过.操作属性或方法, 此时可以称之为函数对象
    2). 方法与属性
        方法是一个特别的属性: 属性值是函数
    3). 方法与函数
        在对象内定义的常称为方法, 通过对象调用的常称为方法, 其它常称为函数
### 比较函数的call()/apply()/bind()
    1). call(obj, param1, param2)/apply(obj, [[param1, param2])
       调用/执行函数
       只是强制指定函数中的this为第一个参数指定的对象
       如果函数执行需要传参数, call是依次传递, apply需要封装成数组传递
    2). bind(obj)
       返回一个新函数, 不会自动执行, 需要手动执行
       新函数内部会通过原函数对象的call来调用原本的函数, 并指定函数的this为obj
       如果直接调用原来函数, this没有绑定为obj
### 详细说明如何判断函数中的this
    1). 正常情况: 执行函数的方式决定了函数中的this
       直接调用: fn()       window
       new调用: new fn()   新创建的对象 
       对象调用: obj.fn()   obj对象
       call/apply调用: fn.call(obj)   第一个参数指定的对象
    2). 特别情况:
       bind()返回的函数: fn2 = fn.bind(obj) fn2()第一个参数指定的对象
       箭头函数: 使用的外部的this(内部没有自己的this) fn = () => {} fn()
       回调函数
          定时器回调/ajax回调/数组遍历相关方法回调: window
          dom事件监听回调: dom元素
          组件生命周期回调: 组件对象
    3). 在开发我们经常会利用箭头函数/bind()来改变this的指向
    
### 关于2个引用变量指向同一个对象的2个问题
    1). 2个引用变量指向同个对象, 通过一个引用变量改变对象内部的数据, 另一个引用变量看到的新的
    2). 2个引用变量指向同个对象, 让一个引用变量指向一个新的对象, 另一个引用变量看到的还是原来的对象
### 内存结构图(原型结构图)
    function Foo () {}  // new Function()
    const fn1 = new Foo()
    const fn2 = new Foo()
    const o1 = {}
    const o2 = new Object()
![](https://i.imgur.com/jcjTXM4.png)
