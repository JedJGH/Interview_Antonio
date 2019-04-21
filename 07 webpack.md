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
### webpack loader和plugin区别(美团)
### babel的插件和预设
    未来版本 ECMAScript 标准经历五个阶段：Strawman（稻草人），Proposal（提议），Draft（草案），Candidate（候选）以及 Finished （完成）
    也就是对应 stage0、stage1、stage2、stage3、stage4 五个阶段
    babel常用的预设包:
        es2015：包含 es2015 语法标准所有相关插件
        es2016：包含 es2016 语法标准所有相关插件
        es2017：包含 es2017 语法标准所有相关插件
        latest：包含从 2015 开始历年语法标准所有相关插件
        env：在 latest 基础上提供环境配置能力，比如可以配置只支持某一个浏览器的某几个版本，会自动按需启用、禁用插件
        stage-0：包含处于标准提案 stage 0 阶段的语法所有相关插件
        stage-1：包含处于标准提案 stage 1 阶段的语法所有相关插件
        stage-2：包含处于标准提案 stage 2 阶段的语法所有相关插件
        stage-3：包含处于标准提案 stage 3 阶段的语法所有相关插件
    预设包与插件包的关系
        一个插件包只能解析1种语法
        预设包是n个插件包的的集合包
    babel的配置: .babelrc
        {
          presets: [], // 配置所有需要的预设包
          plugins: [], // 配置额外需要的插件包
        }
### 组件化编码流程和2个重要问题
    1). 流程
        1. 拆分组件: 拆分界面, 定义组件
        2. 静态组件
        3. 动态组件
           1). 动态初始化显示
           2). 交互
    2). 2个问题
        1. 数据保存在哪个组件?   哪个组件需要还是哪些组件需要
        2. 更新数据的方法定义在哪个组件?   与数据同在一个组件
### 说说读取表达式a.b的值的查找流程
    先查找a, 沿着作用域链查找, 找不到报错(变量未定义)
    找到后查找对象上的b属性, 查找原型链, 如果找不到返回undefined
