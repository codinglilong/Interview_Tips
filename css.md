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