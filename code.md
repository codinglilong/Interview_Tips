# 代码题

## 元素垂直水平居中
> 详情见页面1

1. 需要父盒子子盒子高度，文字水平居中
    ```
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
    ```
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
    ```
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
    ```
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

## 左侧固定右侧自适应
> 详情见页面2

1. 方法一.
    ```
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
    ```
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
    ```
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

## 将下列26字母十进制互转
> 详情见页面3

A=1,B=2,3=C,26=Z,27=AA,28=AB,29=AC...
```
//讲输入的字母传转换为十进制

function strToNumberFor26(str){
    var strArr = str.split('');
    var j = 1;
    var num=0;
    for (let i = strArr.length-1; i >=0 ; i--) {
        var char = strArr[i];
        var c = char.toUpperCase();
        num += (c.charCodeAt(0)-64)*j;
        j*=26;
    }
    return num;
}
console.log(strToNumberFor26('vv'));
//将十进制转换为字符串
function numberToStrFor26(num){
    var str='';
    while(num>0){
        var m =num%26;
        str = String.fromCharCode(m+ 64 )+str;
        num = (num - m) /26;
    }
    return str;
}
console.log(numberToStrFor26(200));
```

## 多维数组转换为一维数组
> 详情见页面4

1. 缺点：转换后数组中的每项为字符串
```
var arr =[1,4,2,[9,2,4,[7,3,2],[4,1]]];
console.log(arr.join(',').split(','));
```

2. 递归
```
var arr =[1,4,2,[9,2,4,[7,3,2],[4,1]]];
var newArr=[];
function fun(arr){
    for(var i =0;i<=arr.length;i++){
        if(Array.isArray(arr[i])){
            fun(arr[i]);
        }else{
            if(arr[i]){
                newArr.push(arr[i]);
            }
        }
    }
}
fun(arr);
console.log(newArr);
```

3. 利用es6和reduce
```
var arr =[1,4,2,[9,2,4,[7,3,2],[4,1]]];
const flatten = arr =>arr.reduce(
    (acc,val)=>acc.concat(Array.isArray(val)?flatten(val):val),[]
);
console.log(flatten(arr));
```

## for循环正确的输出0，1，2
> 详情见页面5

```
for (var i = 0; i < 3; i++) {
    setTimeout(function(){
        console.log(i);
    },1000);
}
//输出3，3，3，正确要输出0，1，2
```
1. 利用es6 let块作用域解决
```
for (let i = 0; i < 3; i++) {
    setTimeout(function(){
        console.log(i);
    },1000);
}
```

2. 利用闭包
```
for (var i = 0; i < 3; i++) {
    (function(i){
        setTimeout(function(){
            console.log(i);
        },1000*i);
    })(i)
}
```

## 冒泡排序
> 详情见页面6

```
var arr=[1,2,3,4,5,6];
function bubbleSort(arr){
    for (let i = 0; i < arr.length-1; i++) {
        var flag=true;
        for (let j = 0; j < arr.length-i-1; j++) {
            if(arr[j]>arr[j+1]){
                var temp=arr[j+1];
                arr[j+1]=arr[j];
                arr[j]=temp;
                flag=false;
            }
        }
        if(flag){
            break;
        }
        
    }
    return arr;
}
console.log(bubbleSort(arr));
```

## 快速排序
> 详情见页面7

```
var arr=[5,8,4,7,2,6];
function quickSort(arr){
    if(arr.length<=1) return arr;
    var middleIndex = Math.floor(arr.length/2);
    var middleValue = arr.splice(middleIndex,1);
    var left = [];
    var right = [];
    for (let i = 0; i < arr.length; i++) {
        if(middleValue >= arr[i]){
            left.push(arr[i])
        }else{
            right.push(arr[i])
        }
        
    }
    return quickSort(left).concat(middleValue,quickSort(right));
}
console.log(quickSort(arr));
```