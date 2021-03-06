## Vue
### 你如何评价vue
    框架能够让我们跑的更快，但只有了解原生的JS才能让我们走的更远。
    vue专注于MVVM中的viewModel层，通过双向数据绑定，把view层和Model层连接了起来。核心是用数据来驱动DOM。
    这种把directive和component混在一起的设计有一个非常大的问题，它导致了很多开发者滥用Directive（指令），
    出现了到处都是指令的情况。
    优点： 
    1.不需要setState，直接修改数据就能刷新页面，而且不需要react的shouldComponentUpdate就能实现最高效的渲染路径。
    2.渐进式的开发模式，模版方式->组件方式->路由整合->数据流整合->服务器渲染。
    3.上手的曲线更加平滑简单，而且不像react一上来就是组件全家桶 
    4.v-model给开发后台管理系统带来极大的便利，反观用react开发后台就是个杯具 4.html，css与js比react更优雅地结合在一个文件上。
    缺点：
    1.指令太多，自带模板扩展不方便； 
    2.组件的属性传递没有react的直观和明显
### Vue虚拟DOM和React虚拟DOM的区别
    Vue: 在渲染过程中，会跟踪每一个组件的依赖关系，不需要重新渲染整个组件树。
    React: 
    * 每当应用的状态被改变时，全部子组件都会重新渲染。 
    * 当某个组件的状态发生变化时，它会以该组件为根，重新渲染整个组件子树。 
    * 如要避免不必要的子组件的重新渲染，要在所有可能的地方使用PureComponent，或是手动实现shouldComponentUpdate方法
    * 数据流是自上而下单向的从父节点传递到子节点，所以组件是简单且容易把握的，子组件只需要从父节点提供的props中获取数据并渲染即可。如果顶层组件的某个prop改变了，React会递归地向下遍历整棵组件树，重新渲染所有使用这个属性的组件。
### 为什么选择vue
    Reactjs 的全家桶方式，实在太过强势，而自己定义的JSX规范，
      揉和在JS的组件框架里，导致如果后期发生页面改版工作，工作量将会巨大。
    Vue的核心：数据绑定 和 视图组件。
    Vue的数据驱动：数据改变驱动了视图的自动更新，传统的做法你得手动改变DOM来改变视图，
      vuejs只需要改变数据，就会自动改变视图，一个字：爽。再也不用你去操心DOM的更新了，这就是MVVM思想的实现。
    视图组件化：把整一个网页的拆分成一个个区块，每个区块我们可以看作成一个组件。网页由多个组件拼接或者嵌套组成    
### 说说vue的生命周期
    1). 初始化
       beforeCreate()
       created()
       beforeMount()
       mounted(): 异步任务(发ajax请求/启动定时器)
    2). 更新
       beforeUpdate()
       updated()
    3). 死亡
       beforeDestroy(): 收尾工作(清除定时器)
       destroyed()    
### 说说你对MVVM的理解
    Model层     代表数据模型，可以在Model中定义数据修改和操作业务逻辑； 
    View层      代表UI组件。负责将数据模型转换成UI展现出来 
    ViewModel层 是一个同步View和Model的对象
    用户操作View层，View数据变化会同步到Model，Model数据变化会立即反应到view中。
    viewModel通过双向数据绑定把view层和Model层连接了起来    
### Vue的MVVM实现结构图
![](http://baocangwh.cn/t6/702/1555977325x2890173947.png)    
### 说说MVVM设计模式
    M: Model(模型), vue中是data(为view提供数据)
    V: View(视图), vue中是模板页面(显示data中的数据)
    VM: ViewModel(视图模型), vue中是Vue实例对象(管理者: 数据绑定/DOM监听) 
### Vue双向绑定底层实现原理
    1). 作用:
       实现数据的更新显示
    2). 基本原理:
        2个核心技术: 数据劫持/监视(Object.defineProperterty()) + 消息订阅与发布(dep与watcher)
       a.通过Object.defineProperterty()给data中所有属性添加setter/getter, 实现数据劫持
       b.为每个data中的属性创建一个对应的dep对象, 一旦属性数据变化, 通知dep对象
       c.为模板中的每个表达式创建对应的watcher, 并关联到对应的dep上
       d.一旦dep收到数据变化的通知, 会通知所有关联的watcher, watcher收到通知后就更新对应的节点
### Vue实现数据双向绑定的原理
![](assets\1556158238222.png)
  
    1). 双向数据绑定是单向数据绑定的基础上的, 所以先简单说清单向数据绑定, 再说双向
    2). vue单向数据绑的实现：
        数据劫持 + 订阅者-发布者模式的方式，
        通过Object.defineProperty()来劫持/监视data中的属性，
          在数据变动时发布消息给所有订阅者，每个订阅者去更新对应的DOM节点。
    3). 双向绑定: 给元素绑定input监听, 一旦输入改变了, 将最新的值保存到对应的属性上    
### 说说数据绑定的理解和基本原理
    1). 作用:
       实现数据的更新显示
    2). 基本原理:  数据劫持 + 订阅者-发布者
       a.在observer中, 通过Object.defineProperterty()给data中所有属性添加setter/getter, 实现数据劫持
       b.为每个data中的属性创建一个对应的dep对象(订阅器)
       c.为模板中的每个表达式创建对应的watcher对象(订阅者), 并关联到对应的dep上
       d.一旦data中的数据发生变化, setter(发布者)会通过dep对象通知所有关联的watcher, watcher收到通知后就更新对应的节点
### 双向绑定和单向数据绑定的优缺点
    单向绑定的优点:
    1.可以带来单向数据流，这样的好处是流动方向可以跟踪，流动单一，没有状态, 
      这使得单向绑定能够避免状态管理在复杂度上升时产生的各种问题, 程序的调试会变得相对容易。
    2.单向数据流更利于状态的维护及优化，更利于组件之间的通信，更利于组件的复用，非UI组件只有单向
    
    双向数据流的优点：
    1. 只有UI控件才存在双向,无需进行和单向数据绑定的那些CRUD（Create，Retrieve，Update，Delete）操作； 
    2. 在一些需要实时反应用户输入的场合会非常方便，用户在视图上的修改会自动同步到数据模型中去，
      数据模型中值的变化也会立刻同步到视图中去；
    缺点：
    双向数据流是自动管理状态的, 但是在实际应用中会有很多不得不手动处理状态变化的逻辑, 
    使得程序复杂度上升 无法追踪局部状态的变化 双向数据流，值和UI绑定，
      但由于各种数据相互依赖相互绑定，导致数据问题的源头难以被跟踪到
    Vue 虽然通过 v-model 支持双向绑定，但是如果引入了类似redux的vuex，就无法同时使用 v-model。
    
    双绑跟单向绑定之间的差异只在于，双向绑定把数据变更的操作隐藏在框架内部，调用者并不会直接感知。
    <input v-model="something">
    <!-- 等价于以下内容 -->
    <input :value="something" @input="something = $event.target.value">
    也就是说，你只需要在组件中声明一个name为value的props，并且通过触发input事件传入一个值，就能修改这个value。    

### 谈谈你对组件的理解
    一个组件应该有以下特征：
    1. 可组合（Composeable）：一个组件易于和其它组件一起使用，或者嵌套在另一个组件内部。
    2. 如果一个组件内部创建了另一个组件，那么说父组件拥有（own）它创建的子组件，通过这个特性，
        一个复杂的UI 可以拆分成多个简单的 UI 组件；
    3. 可重用（Reusable）：每个组件都是具有独立功能的，它可以被使用在多个 UI 场景；
    4. 可维护（Maintainable）：每个小的组件仅仅包含自身的逻辑，更容易被理解和维护；
    5. 可测试（Testable）：因为每个组件都是独立的，那么对于各个组件分别测试显然要比对于整个 UI 进行测试容易的多。
### 组件化编程的基本流程
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
       1). 状态数据保存在哪个组件?  
          看是某个组件需要, 还是某些组件需要(给父组件)
       2). 更新状态数据的行为在哪个组件? 
          状态在哪个组件, 行为就定义在哪个组件
### vue组件通信(美团)
    非父子组件间通信，Vue 有提供 Vuex，以状态共享方式来实现同信，对于这一点，应该注意考虑平衡，
      从整体设计角度去考量，确保引入它的必要。
    父传子: this.$refs.xxx 子传父: this.$parent.xxx
    还可以通过$emit方法出发一个消息，然后$on接收这个消息
### 说说vue组件间通信的几种方式
    1). props
        父子组件间通信的基本方式
        属性值的2大类型: 
            一般: 父组件-->子组件
            函数: 子组件-->父组件
        隔层组件间传递: 必须逐层传递(麻烦)
        兄弟组件间: 必须借助父组件(麻烦)
    2). vue自定义事件
        方式1: 给子组件标签绑定事件监听
            子组件向父组件的通信方式
            功能类似于function props
            不适合隔层组件和兄弟组件间的通信
        方式2: 通过单独的vm对象绑定监听/分发事件
            任意组件间通信
    3). 消息订阅和发布(pubsub-js)
        适合于任何关系的组件间通信
        缺点: 管理不够集中
    4). vuex
        多组件共享状态(数据的管理)
        组件间的关系也没有限制
        功能比pubsub强大, 更适用于vue项目
    5). slot
        父向子通信
        通信是带数据的标签
        注意: 标签是在父组件中解析
### 说说vue项目中如何与后台通信
    1). 通过ajax请求与后台通信
    2). 常用的库
       vue-resource: vue的插件, 用于vue1.x
       axios: 独立的第三方库, 用于vue2.x
       fetch: 较新的原生方式, 但需要引入兼容包: fetch.js
    3). 执行请求代码的时机
       初始化异步显示: mounted()
       特定用户操作后异步显示: 事件回调函数或相关函数中   
### 写出7个指令及其作用
    v-text: 设置标签体文本
    v-html: 设置标签体子标签
    v-if/v-else/v-show: 显示/隐藏
    v-for: 遍历显示列表
    v-bind: 强制绑定表达式, 简写:
    v-on: 绑定事件监听, 简写@
    v-model: 双向数据绑定
### 写出vue7个配置选项及其作用
    el: 最外层元素选择器
    props: 声明接收哪些属性
    data: 状态数据
    computed: 计算属性
    methods: 事件回调函数
    watch: 监视属性变化
    directives: 注册局部指令
    filters: 注册局部过滤器
    components: 局部注册组件
### 说说你对计算属性的理解
    什么时候用计算属性?
       模板显示要显示的数据是根据n个已有的相关数据进行计算来确定
    getter: get方法, 当读取属性值时/相关数据发生了改变自动调用, 根据相关数据进行计算并返回结果, this就是vm
    setter: set方法, 当修改了当前属性的值自动调用, 监视属性值的变化去更新相关数据, this就是vm
### 区别computed与method和watch
    1). computed与method: computed有缓存可以避免重复计算, 而method不可以
    2). computed与watch: 
        get(): 可以监视所有依赖数据, 编码简洁, 但只能同步计算产生一个需要显示的结果数据
        watch: 可以监视所有依赖数据, 编码麻烦, 可以进行异步/长时间处理后更新数据显示
### data中的数组与对象属性不同处理
    数组: 重写数组更新数组元素的方法, 只要调用数组的这些方法, 就会更新相应的界面
    对象: 对对象中的属性进行setter监视, 只要设置了新的属性值, 就会更新相应的界面
### props和v-model
    问题: v-model指向父组件传入的属性, 会导致直接更新父组件的数据, 这违背了组件化开发单向数据流的基本原则
    解决:
        方法1: data + watch
        方法2: 计算属性get + set
### v-show和v-if区别
    与v-if不同的是，无论v-show的值为true或false，元素都会存在于HTML代码中；而只有当v-if的值为true，元素才会存在于HTML代码中
### vue中mixin与extend区别
    全局注册混合对象，会影响到所有之后创建的vue实例，而Vue.extend是对单个实例进行扩展。
    mixin 混合对象（组件复用）
    同名钩子函数（bind，inserted，update，componentUpdate，unbind）将混合为一个数组，因此都将被调用，混合对象的钩子将在组件自身钩子之前调用
    methods，components，directives将被混为同一个对象。两个对象的键名（方法名，属性名）冲突时，取组件（而非mixin）对象的键值对
### vue component和指令的区别(美团)
1
### vm对象与组件对象的关系
    1). 组件对象是Vue的子类型对象, Vue原型对象上的属性/方法所有组件对象都可以访问
    2). 一旦将某个数据或行为添加到Vue原型对象上, 那所有组件中都可通过this轻松访问
    3). 事件总线(EventBus)对象的存储: Vue.prototype.$eventBus = new Vue(), 在组件中直接访问: this.$eventBus

### vue-router的实现原理(美团)
    单页面应用(SPA)的核心之一是: 更新视图而不重新请求页面,目前实现路由的方式有两种：
      ① 通过URL中的hash(‘#’)来实现，
      ② 利用History interface在HTML5中新增的方法(back()、forward()、go()方法)
    vue-router通过配置参数mode来设置，默认是hash模式。
    ```javascript
    import Vue from "vue"
    import vueRouter from "vue-router" //引入vue-router
    import routes from "@/routes" // 引入路由
    Vue.use(vueRouter)  //让Vue使用vuerouter以获得vuerouter的方法
    
    const router = new vueRouter({
      routes,
      mode:"history" //默认是hash ，这里可以改变路由的方式
    })
    export default router
    ```
### vue-router提供了哪些语法?
    1). 1个函数:
        VueRouter: 路由构建函数, 用于创建路由器对象, 配置路由
    2). 2个对象
        $route: 代表当前路由的对象, 包含当前路由相关信息(path, params参数, query参数)
        $router: 代表路由器对象, 包含控制路由跳转的方法(push/replce/back())
    3). 3个标签
        <router-link>: 路由链接, 生成路由链接
        <router-view>: 路由视图, 显示当前路由组件
        <keep-alive>: 缓存路由组件对象
### GET请求的2种请求参数
    1). query参数: 
       路由path: /register
       请求path: /register?username=xxx&password=yyy   
       获取参数: req.query.username
    2). param参数: 
       路由path: /register/:username/:password
       请求path: /register/xxx/123   
       获取参数: req.params.username        

### 说说回调函数的判断及相关问题
    1. 什么函数?
        1). 你定义的
        2). 你没有调用
        3). 但最终执行了(在后面的某个时刻或者某个条件下)
    2. 关于回调函数相关的3个问题
        1). 什么时候调用
        2). 用来做什么的
        3). this是谁
### 说说2种类型的数据容器
    1). 对象
      属性值才是我们要存的数据
      属性名是数据的标识名称, 根据标识名来操作数据
    2). 数组
      数组中的元素就是我们要存的数据
      元素的下标就是数据的标识名称, 根据标识名来操作数据
    3). 选择哪种容器
      一般有序的用数组, 不强调顺序的可用对象
      如果需要根据标识数据查找数据对象, 用对象容器比用数组容器效率高


​       

### 说说你对事件处理机制的理解
    1). DOM事件
       * 绑定事件监听
          * 事件名(类型): 只有有限的几个, 不能随便写
          * 回调函数: 接收包含相关数据的event, 处理事件
       * 用户操作界面自动触发事件(event)
          * 事件名(类型)
          * 数据: event
    2). 自定义事件(如vue自定义事件/pubsub等)
       * 绑定事件监听
          * 事件名(类型): 任意
          * 回调函数: 通过形参接收数据, 在函数体处理事件
       * 触发(emit)/分发(dispatch)事件(编码)
          * 事件名(类型): 与绑定的事件监听的事件名一致
          * 数据: 会自动传递给回调函数
### async/await的作用和使用
    1). 作用?
        简化pormise的使用(不用再使用then()/catch()来指定成功或失败的回调函数)
        以同步编码的方式实现异步流程(没有回调函数)
    2). 哪里使用await?
        在返回promise对象的表达式左侧, 为了直接得到异步返回的结果, 而不是promsie对象
    3). 哪里使用async?
        使用了await的函数定义左侧

### 说说vue的数据代理
    1.通过一个对象(vm)代理对另一个对象(data)中属性的操作(读/写)
    2.好处: 更方便的操作data中的数据
    3.基本实现流程
      1). 通过Object.defineProperty()给vm添加与data对象的属性对应的属性描述符
      2). 所有添加的属性都包含getter/setter
      3). 在getter/setter内部去操作data中对应的属性数据
### 说说vue模板解析
    1). 目的
       实现初始化显示
    2). 整体流程
       1. 将el的所有子节点取出, 添加到一个新建的文档fragment对象中
       2. 对fragment中的所有层次子节点递归进行编译解析处理
       3. 将解析后的fragment添加到el中显示
    3). 编译/解析包含大括号表达式的文本节点: textNode.textContent = value
    4). 编译事件指令: elementNode.addEventListener('eventName', callback)
    5). 编译一般指令: elementNode.xxx = value

### 区别一般的HTTP请求与AJAX请求
    相同点: 都是向服务器提交的http请求
    不同点:             
                       --普通的HTTP--            --ajax请求--
       得到             页面(一般)                json(一般)
       浏览器处理响应    自动显示新数据页面         不会刷新/更新页面, 需要手动处理更新
       数据渲染          服务器端                 浏览器端
       应用类型         多页应用                  单页应用
### 说说debug调试
    1). 调试的目的
         1). 查找bug: 不断缩小可疑代码的范围
         2). 查看程序的运行流程(用于熟悉新接手项目的代码)
    2). 如何开启调试模式
         1). 添加语debugger句: 程序运行前        此方式用打包后才运行的项目
         2). 添加(打)断点: 程序运行前或者过程中   此方式用运行源码js
    3). 如何进行调试操作
         resume: 恢复程序执行(可能执行完或者进入下一个断点处)
         step ove: 单步跳转, 尝试执行完当前语句, 进入下一条(如果内部有断点, 自动进入内部断点处)
         step into: 跳入, 进入当前调用函数内部
         step out: 跳出, 一次性执行完当前函数后面所有语句,并出去
         deactivate breakpoints: 使所有断点暂时失效
         call stack: 显示是程序函数调用的过程
         scope: 当前执行环境对应的作用域中包含的变量数据
         breakpoints: 断点列表

### 写出package.json的基本结构
    {
       "name": "vue", // 标识名称
       "version": "1.0.0", // 版本号
       "scripts": { // 打包运行相关的npm命令
          "xxx": "具体命令"   npm run xxx
       },
       dependencies: {}, // 运行时依赖
       devDependencies: {} // 开发时依赖
    }

