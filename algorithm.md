# 算法题


## 多维数组转换为一维数组

1. 缺点：转换后数组中的每项为字符串

    ```javascript
    var arr =[1,4,2,[9,2,4,[7,3,2],[4,1]]];
    console.log(arr.join(',').split(','));
    ```

2. 递归

    ```Javascript
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

    ```Javascript
    var arr =[1,4,2,[9,2,4,[7,3,2],[4,1]]];
    const flatten = arr =>arr.reduce(
        (acc,val)=>acc.concat(Array.isArray(val)?flatten(val):val),[]
    );
    console.log(flatten(arr));
    ```

## 冒泡排序

```js
var arr=[7,4,8,3,1];
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

```js
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



## 判断一个数值接近数组中的某个值

```js
var arr = [3,6,80,9,54,11,5]
function limit(arr,num){
    var newArr = [];
    arr.forEach(i=>newArr.push(Math.abs(i-num)));
    var index = newArr.indexOf(Math.min.apply(null,newArr));
    return arr[index];
}
//第二种
function limit(arr,num){
  let newArr = arr.map(i=>Math.abs(i-num))
  const index = newArr.indexOf(Math.min(...newArr))
  return arr[index]
}

console.log(limit(arr,10));
```



## 数组乱序

```js
const shuffe = ([...arr])=>{
    let m = arr.length;//数组长度
    while(m){
        const i = Math.floor(Math.random()*m--);//随机拿到一个index
        [arr[m], arr[i]] = [arr[i], arr[m]];//逐次交换index中的位置
    }
    return arr;
}
```



## 斐波那契数列

1. 第一种有性能问题最简单方式

    ```js
    function fb(n){
      if(n<=1){
        return n;
      }
      return fb(n-1) + fb(n-2)
    }
    ```

2. 第二种动态规划

    ```js
    function fb(n){
      let arr = Array(n+1).fill(null);
      arr[0]=0;
      arr[1]=1;
      for(let i =2;i<arr.length;i++){
        arr[i] = arr[i-1] + arr[i-2];
      }
      return arr[n]
    }
    ```

3. 最简洁

    ```js
    function fb(n,res1=1,res2=1){
      if(n<=2){
        return res2;
      }
      return fb(n-1,res2,res1+res2)
    }
    ```

4. 效率最好的

    ```js
    function fb(n){
      let res1=0;
      let res2=1;
      let sum= res1+res2;
      for(let i =2; i<=n; i++){
        sum=res1 +res2;
        res1=res2;
        res2=sum;
      }
      return sum
    }
    ```



## 约瑟夫环

> 编号为1到100的一百个人围成一圈，以123 123的方式进行报数，数到3的人自动退出圈子，剩下的人继续报数，问最后剩下的人编号为几？

1. 解法一。

   ```js
   const numbers = Array.from({ length: 100 }, (v, k) => k + 1);
   function fn(n, m) {
     let index = 0;
     while (n.length > 2) {
       for (let i = 1; i <= m; i++) {
         if (i === m) {
           n.splice(index, 1);
         } else {
           if (index > n.length) {
             index = 0;
           } else {
             index++;
           }
         }
       }
     }
   }
   fn(numbers,3)
   ```

2. 解法二数学公式

   > n = 100人，m=3 报数到的号;f(N,M)

   + 1人时候： 0   										最终存活的人的下标为0
   + 2人时候：(0+3)%2 = 1                          最终存活的人的下标为1
   + 3人时候：(1+3)%3 = 1                          最终存活的人的下标为1
   + 4人时候：(1+3)%4 = 0                          最终存活的人的下标为0
   + 5人时候：(0+3)%5 = 3                          最终存活的人的下标为3
   + 6人时候：(3+3)%6 = 0                        最终存活的人的下标为0

   所以得到结果是公式结果为：`f(n,m) = (f(n-1,m)+m)%n`
   
   ```js
    const numbers = Array.from({ length: 100 }, (v, k) => k + 1);
    function fn(n, m) {
      let previous = 0; // 上一次结算的结果
      for (let i = 1; i <= n; i++) {
        previous = (previous + m) % i
      }
      return numbers[previous]
    }
    console.log(fn(numbers.length, 3))
   ```
   



## 将下列26字母十进制互转

A=1,B=2,3=C...26=Z,AA=27,AB=28,AC=29...

```javascript
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

200
7 + 64 = 73 = i
200-7/26 = 193/26=7
7
```