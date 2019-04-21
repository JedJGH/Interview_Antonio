## CSS
### 1.box-sizing(美团)
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
### 5.BFC（Block Formatting Contexts）块级格式化上下文(美团)
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
### 伪类和伪元素的区别(美团) (饿了么)
    CSS 伪类：a:link :first-child :nth-child :focus :visited
    伪类用于当已有元素处于的某个状态时，为其添加对应的样式，这个状态是根据用户行为而动态变化的。，逻辑上存在但在文档树中却无须标识的“幽灵”分类 
    CSS 伪元素（:first-letter，:first-line,:after,:before）代表了某个元素的子元素，这个子元素虽然在逻辑上存在，但却并不实际存在于文档树中。 CSS3标准要求伪元素使用双冒号

### 6. postison的几种属性()
### 7. rem移动端
