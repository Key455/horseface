---
title: BFC(块级格式化上下文)
date: 2020-06-23 20:05:03
tags:
banner_img: /img/bg.jpg
index_img: /img/bg.jpg
---
# BFC(块级格式化上下文)

它是页面 `CSS` 视觉渲染的一部分，**用于决定块级盒的布局及浮动相互影响范围的一个区域**。

### BFC 的创建

以下元素会创建 `BFC`：

- 根元素（`html`）
- 浮动元素（`float` 不为 `none`）
- 绝对固定定位元素（`position` 为 `absolute` 或 `fixed`）
- 表格的标题和单元格（`display` 为 `table-caption`，`table-cell`）
- 匿名表格单元格元素（`display` 为 `table` 或 `inline-table`）
- 行内块元素（`display` 为 `inline-block`）
- `overflow` 的值不为 `visible` 的元素

### BFC 的范围

> A block formatting context contains everything inside of the element creating it, that is not also inside a descendant element that creates a new block formatting context.

- 直译过来就是, `BFC`包含创建它的元素的所有子元素, 但不包括创建了新`BFC`的子元素的内部元素
- 简单来说，子元素如果又创建了一个新的 `BFC`，那么它里面的内容就不属于上一个 `BFC` 了，这体现了 `BFC` **隔离** 的思想
- 也就是所说，**一个元素不能同时存在于两个 BFC 中**。

### BFC 的特性

`BFC` 除了会创建一个隔离的空间外，还具有以下特性：

- `BFC` 内部的块级盒会在垂直方向上一个接一个排列

- 同一`BFC`下的相邻块级元素可能发生外边距折叠，创建新的`BFC`可以避免外边距折叠

  ![img](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg1isbqqiaj30og07sgle.jpg)

  ```html
  <style>
      .top {
        height: 50px;
        margin: 10px 0;
        background: yellow;
      }
  
      .outer {
        background: gray;
        height: 50px;
        /* 创建新的 BFC 就可以避免这两个相邻块级盒间的外边距折叠，尝试去掉注释后看效果 */
        /* overflow: auto; */
      }
  
      .inner {
        height: 20px;
        margin: 10px 0; /* 这里设置了inner box的 margin，很明显没有生效 */
        background: green;
      }
  </style>
  <div class="top">TOP</div>
  <div class="outer">
    <div class="inner">INNER</div>
  </div>
  ```

- 每个元素的外边距盒（margin box）的左边与包含块边框盒（border box）的左边相接触（从右向左的格式化，则相反），即使存在浮动也是如此

  ![img](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg1iua5y8jj30di07edfl.jpg)

  ```html
  <style>
      .box {
        background: gray;
      }
  
      .left {
        /* 即使存在浮动元素，BFC中其他元素的margin box的左边也会与包含块border box的左边相接触 */
        /* 在这个例子中，黄框向左浮动，脱离了普通流，此时绿框被定位到包含块的左上角 */
        float: left;
        width: 100px;
        height: 80px;
        background: yellow;
        opacity: .5;
      }
  
      .right {
        width: 200px;
        height: 50px;
        background: green;
        opacity: .5;
      }
  </style>
  <div class='box'>
    <div class="left"></div>
    <div class="right"></div>
  </div>
  ```

- 浮动盒的区域不会和`BFC`重叠

  ![img](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg1itp8m7uj30hq06ygld.jpg)

  ```html
  <style>
      .wrapper {
        height: 100px;
        background: gray;
      }
  
      .float {
        float: left;
        width: 80px;
        height: 100px;
        background: green;
        opacity: .6;
      }
  
      .bfc {
        width: 200px;
        height: 50px;
        background: yellow;
        /* 如果不创建 BFC，浮动盒的区域会和黄框重叠 */
        /* 尝试去掉 overflow 注释，使黄框创建 BFC，观察 BFC 区域和浮动盒的重叠情况 */
        overflow: hidden; 
      }
  </style>
  <div class="wrapper">
    <div class="float"></div>
    <div class="bfc">
    </div>
  </div>
  ```

- 计算`BFC`的高度时，浮动元素也会参与计算

  ```html
  <style>
      .bfc {
        /* 计算 BFC 高度时，包含 BFC 内所有元素，即使是浮动元素也会参与计算 */
        /* 这一特性可以用来解决浮动元素导致的高度塌陷 */
        /* 如果父元素没有创建 BFC，在计算高度时，浮动元素不会参与计算，此时就会出现高度塌陷 */
        overflow: auto;
        background: gray;
      }
  
      .float {
        float: left;
        width: 100px;
        height: 80px;
        background: green;
      }
  
      .inner {
        height: 50px;
        background: yellow;
      }
  
  </style>
  <div class="bfc">
    <div class="float"></div>
    <div class="inner"></div>
  </div>
  ```

### BFC 的应用

- 自适应多栏布局

  中间栏创建 `BFC`，左右栏宽度固定后浮动。由于盒子的 margin box 的左边和包含块 border box 的左边相接触，同时浮动盒的区域不会和 `BFC` 重叠，所以中间栏的宽度会自适应

  ![img](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg1j5tpwutj327m0bwq2r.jpg)

  ```html
  <style>
      .wrapper div {
        height: 200px;
      }
  
      .left {
        float: left;
        width: 200px;
        background: gray;
      }
  
      .right {
        float: right;
        width: 200px;
        background: gray;
      }
  
      .main {
        /* 中间栏创建 BFC */
        /* 由于 盒子的 margin box 的左边和包含块 border box 的左边相接触 */
        /* 同时 BFC 区域不会和浮动盒重叠，所以宽度会自适应 */
        overflow: auto;
        background: cyan;
      }
  </style>
  <div class="wrapper">
     <div class="left"></div>
     <div class="right"></div>
     <div class="main"></div>
  </div>
  ```

- 防止外边距折叠

  创建新的 `BFC` ，让相邻的块级盒位于不同 `BFC` 下可以防止外边距折叠

- 清除浮动

  `BFC` 内部的浮动元素也会参与高度计算，可以清除 `BFC` 内部的浮动

### IFC的规则

- 在一个行内格式化上下文中，盒是一个接一个**水平**放置的，从包含块的顶部开始
- 这些盒之间的**水平**`margin`，`border`和`padding`都有效
- 盒可能以不同的方式竖直对齐：以它们的底部或者顶部对齐，或者以它们里面的文本的基线对齐