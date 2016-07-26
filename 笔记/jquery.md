# jquery笔记
##引入jquery： 
<script type="text/javascript" src="相对路径"></script>
     (注：末尾引入，加载的时候能够找到选择器，实现js行为)

## 选择器$("选择器"):id class element *
##1.事件方法
+ click()
+ mouseenter()
+ mouseleave()
+ mouseover()
+ mouseout()
+ mosemove() 
+ blur()
+ focus()
+ delegate()  向匹配元素的当前或未来的子元素附加一个或多个事件处理器
+ event.target   触发该事件的 DOM 元素。
+ resize()   触发、或将函数绑定到指定元素的 resize 事件
+ scroll()    触发、或将函数绑定到指定元素的 scroll 事件

## 2.属性节点处理、元素内容处理、表单元素处理 
+ 添加属性更改属性 .attr()
+ 改变css属性 .css("属性名"，"属性值")或.css({"属性名"："属性值","属性名":"属性值"})
+ 删除属性 .removeAttr() ;
+ 添加删除Class类：.addClass() 添加 .removeClass() 删除
+ 设置匹配html内容，.html();
+ 改变和获取每个元素的文本内容：.text();
+ 处理表单元素的值：.val();
+ 获取元素的索引值 .index()

## 3.DOM树遍历(找对象,作用对象)
+ .append() 插入后边（插入到子集）
+ .prepend() 插入到前边（插入到子集）
+ .after() 插入到自己的前边（同级别）
+ .before() 插入到自己的前边（同级别）
+ .empty() 删除匹配的集合中的每个元素节点 例:$("div").empty();
+ .remove() 删除节点；
+ .clone() 克隆元素节点
+ .replaceAll() 替换全部元素

## 5.JQuery动画
