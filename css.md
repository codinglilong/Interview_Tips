# css面试宝典

## 清楚浮动

1. clear清除浮动。添加空div法`clear:both;`。缺点需要多写个div
2. 给浮动元素父级设置高度。缺点：在浮动元素高度不确定的时候不适用
3. 以浮制浮（父级同时浮动）。缺点：需要给每个浮动元素父级添加浮动，浮动多了容易出现问题。
4. 给父级添加overflow:hidden 清浮动方法；
5. 万能清除法 after伪类 清浮动

```css
选择符:after{
    content:".";
    clear:both;
    display:block;
    height:0;
    overflow:hidden;
    visibility:hidden;
}
```

## 四种定位的区别

1. static：静态定位，默认定位，不受top,left等属性的控制。
2. relative：相对定位，处在正常的文档流中，但是能被top、left属性控制，以他原来的位置为基准偏移。
3. absolute：绝对定位，脱离文档流，根据父容器为基准进行偏移。如果父容器没有设置position为relative和absolute，那么偏移是以body为依据。
4. fixed：固定定位，脱离文档流，始终是以body为依据，元素会一直固定在这个位置。窗口滚动不会改变位置。

## css层叠上下文

```text
负z-index < 块 < 浮动 < 行内块 < 行内 < 定位 < z-index:auto或者z-index:0 < 大于0的z-index
```

## css盒模型

1. margin
2. border
3. padding
4. content

## 标准模型和IE模型的区别

1. 标准模型width宽度是content内容的宽度
2. IE模型width宽度是 border+padding+content的宽度

## css如何设置这两种模型

1. 标准盒模型：`box-sizing:content-box;`
2. IE模型:`box-sizing:border-box;`

## js如何设置获取盒模型对应的宽和高

1. `dom.style.width/height`只能取值内联，外联的无法取到
2. `dom.currentStyle.width/height`只支持IE
3. `window.getComputedStyle(dom).width/height`chorem/ff
4. `dom.getBoundingClientRect().width/height`

## 盒模型的边距重叠

边界重叠是指两个或多个盒子(可能相邻也可能嵌套)的相邻边界(其间没有任何非空内容、补白、边框)重合在一起而形成一个单一边界。

两个或多个块级盒子的垂直相邻边界会重合。结果的边界宽度是相邻边界宽度中最大的值。如果出现负边界，则在最大的正边界中减去绝对值最大的负边界。如果没有正边界，则从零中减去绝对值最大的负边界。注意：相邻的盒子可能并非是由父子关系或同胞关系的元素生成。 

## BFC的基本概念

块级格式化上下文，是web页面中盒模型布局的CSS渲染模式，指一个独立的渲染区域或者说是一个隔离的独立容器

## BFC的原理和特性

1. 内部的盒子会在垂直方向上一个接一个的放置。
2. 垂直方向上的距离由margin决定
3. bfc区域不会与float的元素区域重叠
4. 计算bfc的高度时，浮动元素也参与计算
5. bfc就是页面上的一个独立容器，容器里面的子元素不会影响外面的元素

## 如何创建BFC

1. 浮动元素，`float`除`none`以外的值
2. 定位元素,`position`的值不为`static`或者`relative`
3. `display`为以下其中之一的值 `table-cell`, `table-caption`, `inline-block`, `flex`, 或者 `inline-flex`.
4. `overflow`的值不为`visible`