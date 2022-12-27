- [You Don't Know CSS](https://zhuanlan.zhihu.com/p/23829153)
# positioning scheme (布局方案)
  - normal flow，其包含三种formatting contexts：block，inline和 relative -  formatting context
  - floats:其与normal flow互动，其构成了现代CSS grid framework基础
  - absolute positioning，其处理absolute 和fixed元素相对于normal flow的定位

# bfc 理解
-  normal flow里的box属于一个formatting context,其是block或者inline，但不能同时都是。block-level盒子参与bfc(block formatting context)，inline-level盒子参与ifc(inline formatting context)
-  处于同一bfc下的盒子才可能发生margin collapse

# margin collapse触发条件
- 两者都为处于同一bfc下的 in-flow block-level boxes
- 两者间不存在line boxes,clearance,padding,border
- 都处于垂直相邻的盒子边缘