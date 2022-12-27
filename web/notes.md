
# css
## 伪类选择器：
nth-child(na+b), nth-of-type(na+b)

## 行内元素中间有空格原因
- 空白节点text
- 默认white-space: normal, 合并空白符

### vertical-align 和 line-height
 空白节点导致图片高度问题
- 默认vertical-align: baseline
- vertical-align起作用前提条件：内联元素以及display值为table-cell的元素

## margin重叠的条件
- 相邻block元素的上或者下margin
- 两者间不存在line boxes,clearance,padding,border

## containing block
- 包含块根据元素的position属性决定
1. position是static, relative, or sticky时，containing block是最近祖先元素的padding box边缘
2. position是absolute时， containing block是最近具有定位属性的父元素的padding box边缘
3. position是fixed时， containing block是viewport
4.  position是absolute 或者 fixed时，containing block是满足下列条件的最近祖先元素padding box边缘
  - transform或者perspective值不为none
  - will-change值是transform或者perspective
  - filter的值不是none，或者will-change值是filter
  - contain: paint;

The `height`, `top`, and `bottom` properties compute percentage values from the `height`of the containing block.
The`width`,`left`,`right`, `padding`, and `margin` properties compute percentage values from the `width`of the containing block.

## CSS选择器的解析
- 解析规则，从右向左



# js
## 动画
使用requestAnimationFrame管理动画刷新时间
 - 大多数电脑显示器的刷新频率是60HZ，大概相当于每秒钟重绘60次。因此，最佳动画循环间隔1000ms/60  = 16.6ms
-  requestAnimationFrame比setTimeout 有更好的性能（契合显示器刷新频率）和节约电池使用（大多数浏览器将停止调用，当运行在后台的tab和隐藏的iframe中时）

## 变量声明提升
在JavaScript代码运行之前其实是有一个编译阶段的。编译之后才是从上到下，一行一行解释执行。变量提升就发生在编译阶段，它把变量和函数的声明提升至作用域的顶端。（编译阶段的工作之一就是将变量与其作用域进行关联）。

变量提升需要注意：

- 在js中用var ,function声明的变量都将被提到函数的最顶部。（但是不会初始化）
- 函数声明的优先级大于变量声明的优先级（function > var）
- 在函数内部变量提升的优先级会小于函数参数（函数参数 > 函数内部变量提升）
- 对于函数声明来说，如果定义了相同的函数变量声明，后定义的声明会覆盖掉先前的声明

##  load 和 DOMContentLoaded
- DOMContentLoaded dom加载完成，外部引用资源，图片等不一定加载完成
- load 所有资源都加载完成

## 节流和防抖
节流（前置执行）和防抖（后置执行）

## [js宽高](https://www.jianshu.com/writer#/notebooks/13629282/notes/73972058/preview)


## 编程范式
- 面向对象编程
面向对象编程的核心在于抽象，提供清晰的对象边界。结合封装、集成、多态特性，降低了代码的耦合度，提升了系统的可维护性。
- 函数式编程
[参考](https://www.zhihu.com/question/28292740?sort=created)
函数式编程的核心在于“避免副作用”，不改变也不依赖当前函数外的数据。

# HTTP
## HTTP1.0, HTTP1.1和HTTP2.0的区别
1. 长链接，HTTP1.1支持长连接，在一个TCP连接上可以传送多个HTTP请求和响应，减少了建立和关闭连接的消耗和延迟，在HTTP1.1中默认开启长连接keep-alive。HTTP1.0需要使用keep-alive参数来告知服务器端要建立一个长连接。
2. 节约带宽。 1.0 不支持断点续传。
3. 多路复用。HTTP2.0使用了多路复用的技术，做到同一个连接并发处理多个请求
4. 头部数据压缩。HTTP1.1不支持header数据的压缩，HTTP2.0使用HPACK算法对header的数据进行压缩，这样数据体积小了，在网络上传输就会更快。
5. 服务器推送。 HTTP2.0引入了server push，它允许服务端推送资源给浏览器

## cookie和token都存放在header里面，为什么只劫持前者？
1. cookie是带状态的而token是无状态的