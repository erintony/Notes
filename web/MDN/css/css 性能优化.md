# 分析
1. css是阻塞式渲染，即在css全部解析完成后才会渲染
2. 浏览器在下载完css文件，构建CSSOM后，paint页面。
3. 浏览器按照一个特定的渲染路径：
  - layout结束后才会paint
  - layout在渲染树构建完成之后
  - 渲染树构建需要DOM和CSSOM
因此，css优化可以从，以上几点出发：
1. 优化CSSOM构建
2. 删除不必要的样式
3. 分隔css文件为当前页面需要的和不需要的，减少css render blocking

# Render Blocking 优化
通过媒体查询引入分割后的css文件
```html
<link rel="stylesheet" href="styles.css"> <!-- blocking -->
<link rel="stylesheet" href="print.css" media="print"> <!-- not blocking --> 
<link rel="stylesheet" href="mobile.css" media="screen and (max-width: 480px)"> 
```
# Animating on the GPU
- 浏览器对css动画进行了优化，处理动画的属性，不会触发reflow。
- 为了提升性能，应用动画的节点可以从主线程移动到GPU。
- 一些属性会导致合成层，`transform, position: fixed, poacity, will-change, filter`. 一些标签`<canvas>, <iframe>, <video>`也在他们自己的图层。
- 当一个元素被提升为一个图层，动画的转换属性将在GPU完成。

# `will-change` 属性
will-change属性告诉浏览器，元素的哪些属性将会改变，然后，浏览器就可以在元素改变之前做一些优化

#  `font-display` 属性
```css
@font-face {
  font-family: someFont;
  src: url(/path/to/fonts/someFont.woff) format('woff');
  font-weight: 400;
  font-style: normal; 
  font-display: fallback; 
}
```
允许字体在加载完成之前，显示备用字体

# `contain`属性
contain属性指出一个元素和他的内容，尽可能的独立与文档树。这将允许浏览器重新布局时，只更新有限的区域，而不是整个文档