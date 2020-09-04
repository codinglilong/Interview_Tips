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
5. sticky:粘性定位。相对定位和固定定位的混合。元素在跨越特定阈值前为相对定位，之后为固定定位。这个特定阈值指的是top,right,bottom,left之一，必须指定这四个阈值其中之一，才可使粘性定位生效。否则其行为与相对定位相同。


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



## BFC 主要被用来解决以下常见的布局问题：

- 清除浮动；
- 阻止 margin 发生重叠；
- 阻止元素被浮动的元素覆盖。



## css预处理提供了哪些好处

1. 嵌套代码的能力，通过嵌套来反映不同css属性之间的层级关系
2. 支持定义css变量
3. 提供计算函数
4. 允许对代码片段进行extend和mixin
5. 支持循环语句的使用
6. 支持将css文件模块化，实现服用



## 元素垂直水平居中

> 详情见页面1

1. 需要父盒子子盒子高度，文字水平居中

   ```css
   .box{
       position: relative;
       height:100%
   }
   .box .content{
       position: absolute;
       margin: auto;
       background-color: red;
       top: 0;
       left: 0;
       right: 0;
       bottom: 0;
       text-align: center;
       width: 300px;
       height: 350px;
   }
   ```

2. 利用css3 transform的translate属性来定位,不需要子盒子高度，但是需要父盒子高度

   ```css
   .box{
       position: relative;
       height:100%;
   }
   .box .content{
       background-color: red;
       width:100px;
       height:100px;
       position:absolute;
       top:50%;
       left:50%;
       transform: translate(-50%,-50%);
   }
   ```

3. 需要设置子盒子宽高，然后margin-left和margin-top减去子盒子的宽高，需要计算

   ```css
   .box {
       position: relative;
       height: 100%;
   }
   
   .box .content {
       width: 100px;
       height: 100px;
       position: absolute;
       top: 50%;
       left: 50%;
       margin: -50px 0 0 -50px;
       background-color: red;
   }
   ```

4. 利用css3 flex弹性盒子来实现，最为简洁

   ```css
   .box {
       display: flex;
       justify-content: center;
       align-items:center;
       height: 100%;
   }
   
   .box .content {
       background-color: red;
   }
   ```

## 两边固定中间自适应

> 详情见页面10

全局样式

```html
<style>
    *{
        padding: 0;
        margin: 0;
        height: 100%;
    }
    .left,.right,.center{
        height: 200px;
    }
    .left,.right{
        width: 300px;
    }
    .left{
        background-color: yellow;
    }
    .right{
        background-color: skyblue;
    }
</style>
```

1. 方法一：绝对定位

   ```html
   <section>
       <style>
           .left,.right{
               position: absolute;
               top: 0;
           }
           .left{
               left: 0;
           }
           .right{
               right: 0;
           }
           .center{
               margin: 0 300px;
           }
       </style>
       <article class="layout-position-margin">
           <div class="left"></div>
           <div class="center">
               <h1>1.绝对定位</h1>
           </div>
           <div class="right"></div>
       </article>
   </section>
   ```

2. 方法二：浮动布局

   ```html
   <section>
       <style>
           .left{
               float: left;
           }
           .right{
               float:right;
           }
           .center{
               margin: 0 300px;
           }
       </style>
       <article class="layout-float">
           <div class="left"></div>
           <div class="right"></div>
           <div class="center">
               <h1>2.浮动布局</h1>
           </div>
       </article>
   </section>
   ```

3. 方法三：flex布局

   ```html
   <section>
       <style>
           .layout-flex{
               display:flex;
           }
           .center{
               flex: 1;
           }
           .left,.right{
               flex-shrink: 0; //不让left和right缩放
           }
       </style>
       <article class="layout-flex">
           <div class="left"></div>
           <div class="center">
               <h1>3.flex布局</h1>
           </div>
           <div class="right"></div>
       </article>
   </section>
   ```

4. 方法四：table布局

   ```html
   <section>
       <style>
           .layout-table{
               display:table;
               width: 100%;
           }
           .layout-table>div{
               display: table-cell
           }
       </style>
       <article class="layout-table">
           <div class="left"></div>
           <div class="center">
               <h1>4.table布局</h1>
           </div>
           <div class="right"></div>
       </article>
   </section>
   ```

5. 方法五：grid布局

   ```html
   <section>
       <style>
           .layout-gird{
               display: grid;
               width: 100%;
               grid-template-rows: 300px;
               grid-template-columns: 300px auto 300px;
           }
       </style>
       <article class="layout-gird">
           <div class="left"></div>
           <div class="center">
               <h1>5.网格布局</h1>
           </div>
           <div class="right"></div>
       </article>
   </section>
   ```

6. 方法六：圣杯布局

   ```html
   <section>
       <style>
           .layout-sb{
               padding: 0 300px;
           }
           .layout-sb>div{
               float: left;
               position: relative;
           }
           .left{
               left: -300px;
               margin-right: -300px;
           }
           .right{
               right: -300px;
               margin-left: -300px;
           }
           .center{
               width: 100%;
           }
       </style>
       <article class="layout-sb">
           <div class="left"></div>
           <div class="center">
               <h1>6.网格布局</h1>
           </div>
           <div class="right"></div>
       </article>
   </section>
   ```

7. 方法七：双飞翼布局

   ```html
   <section>
       <style>
           .layout-sfy>div{
               float: left;
           }
           .left{
               margin-left: -100%;
           }
           .right{
               margin-left: -300px;
           }
           .center{
               width: 100%;
           }
           .center .main{
               margin: 0 300px;
           }
       </style>
       <article class="layout-sfy">
           <div class="center">
               <div class="main">
                   <h1>7.双飞翼布局</h1>
               </div>
           </div>
           <div class="left"></div>
           <div class="right"></div>
       </article>
   </section>
   ```

### 双飞翼布局的好处

1. 主要的内容先加载
2. 兼容目前所有的主流浏览器，包括IE6在内
3. 实现不同的布局方式，可以通过调整相关css属性即可实现

## 左侧固定右侧自适应

> 详情见页面2

1. 方法一.

   ```css
   .box .left{
       height: 300px;
       width: 400px;
       background-color:red;
       float: left;
   }
   .box .right{
       height: 300px;
       margin-left: 400px;
       background-color:rebeccapurple;
   }
   ```

2. 利用css3 中的计算属性

   ```css
   .box .left{
       height: 300px;
       width: 400px;
       background-color:red;
       float: left;
   }
   .box .right{
       float: left;
       height: 300px;
       width:calc(100% - 400px);
       background-color:rebeccapurple;
   }
   ```

3. 利用css3 中的flex布局

   ```css
   .box{
       display: flex;
   }
   .box .left{
       height: 300px;
       width: 400px;
       background-color:red;
       flex: 0 0 400px;
   }
   .box .right{
       flex: 1;
       height: 300px;
       background-color:rebeccapurple;
   }
   ```

4. 利用后css3中的grid布局

   ```css
   .box{
       display: grid;
       grid-template-columns: 400px auto;
   }
   .box .left{
       height: 300px;
       background-color:red;
   }
   .box .right{
       height: 300px;
       background-color:rebeccapurple;
   }
   ```

## CSS3实现一个扇形

> 详情见页面8

```css
.box{
    width: 0;
    height: 0;
    border-width: 50px;
    border-style: solid;
    border-color: #f00 transparent transparent;
    border-radius: 50px;
}
```



## 重排和重绘

> DOM的变化影响了宽高和定位，比如改变了宽度或者给段落增加了文字，导致行数增加，浏览器需要重新计算元素的几何属性，同时其他元素也会受到本次本次操作的影响，浏览器就会重新构造渲染树，这就叫做 **重排**，完成重排之后，浏览器会重新绘制受到影响的部分到屏幕中，这叫 **重绘**，发生了重排必定需要重绘

1. 重排情况
   + 元素位置发生了变化
   + 元素尺寸发生了变化
   + 添加或者删除可见的DOM元素
   + 内容改变
   + 页面渲染初始化的时候
   + 浏览器窗口尺寸改变
2. 重绘情况
   + 改变了字体颜色
   + 背景颜色