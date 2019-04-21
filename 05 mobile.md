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
### em和rem(美团)
