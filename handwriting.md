# js手写题

## 简单深拷贝

```js
function deepClone(obj) {
  if (typeof obj !== 'object' || obj === null) {
    return obj;
  }
  let newObj;
  if (Array.isArray(obj)) {
    newObj = [];
  } else {
    newObj = {};
  }
  for (let item in obj) {
    if (obj.hasOwnProperty(item)) {
      newObj[item] = deepClone(obj[item])
    }
  }
  return newObj
}
```

## Object.create实现

```js
Object.prototype.create = function(proto){
    function F(){}
    F.prototype = proto;
    return new F();
}

```

## bind

```js
Function.prototype.bind=function(context){
  context = context || window;
  const self = this; //保存原函数
  const args = [].slice.call(arguments,1); //把参数变为数组进行处理
  return function(){
    const selfArgs = [].slice.call(arguments);// 把返回后的函数参数变为数组
    return self.apply(context,[].concat(args,newArgs)) //执行保存的原函数并且绑定this，合并参数
  }
}
```

## call两种方式

1. 第一种不使用高级api

    ```js
    Function.prototype.call = function (context) {
      context = context || window;
      context.fn = this;
      let args = [];
      for (let i = 1; i <= arguments.length; i++) {
        args.push('arguments[' + i + ']');
      }
      const result = eval('context.fn(' + args + ')');
      delete context.fn;
      return result
    }
    ```

2. 第二种

    ```js
    Function.prototype.call = function(context){
      context = context || window;
      context.fn = this;
      let args = [...arguments].slice(1);
      const result = context.fn(...args);
      delete context.fn;
      return result
    }
    ```

## 手写apply

1. 第一种

    ```js
    Function.prototype.apply1 = function (context, params) {
      context = context || window;
      context.fn = this;
      const result = context.fn(...params);
      delete context.fn;
      return result;
    ```

2. 第二种

    ```js
    Function.prototype.apply1 = function (context, params) {
      context = context || window;
      context.fn = this;
      let args = [];
      if (params instanceof Array) {
        for (let i = 0; i < params.length; i++) {
          args.push('params[' + i + ']')
        }
      }
      const result = eval('context.fn(' + args + ')');
      delete context.fn;
      return result
    }
    ```

## 节流函数 throttle

> 连续触发事件但是在 n 秒中只执行一次函数

1. 第一种利用时间戳立即执行,缺点是最后的输入无法捕捉

   ```js
   function throttle(delay,fn){
      let previous = 0;
      return function(){
        let now = Date.now();
        if(now - previous > delay){
          fn.apply(this,arguments);
          previous = now;
        }
      }
    }
   ```

2. 第二种不能立即执行

    ```js
    function throttle(delay,fn){
      const timer;
      return function(){
        if(timer) return;
        const self = this;
        timer = setTimeout(function(){
          timer = null;
          fn.apply(self,arguments);
        },delay);
      }
    }
    ```

3. 第三种立即执行，并且最后一次也能正确执行

    ```js
    function throttle(delay,fn){
      let previous=0;
      let now = 0;
      let timer;
      return function(){
        now =+ Date.now();
        if(now-previous > delay){
          if(timer){
            clearTimeout(timer);
            timer = null;
          }
          fn.apply(this,arguments);
          previous = now;
        }else if(!timer){
          const self=this;
          timer = setTimeout(function(){
            timer = null;
            fn.apply(self,arguments);
            previous = now;
          },delay)
        }
      }
    }
    ```

## 防抖函数 debounce

> 触发事件后在 n 秒内函数只能执行一次，如果在 n 秒内又触发了事件，则会重新计算函数执行时间

1. 第一种不能立即执行

    ```js
    function debounce(delay,fn){
      let timer;
      return function(){
        clearTimeout(timer);
        const self = this;
        timer = setTimeout(function(){
          fn.apply(self,arguments);
        },delay)
      }
    }
    ```

2. 第二种够用

   ```js
   function debounce(delay,fn){
     let timer;
     return function(){
       if(!timer){ //首次进入执行
         fn.apply(this,arguments);
       }
       const self = this;
       clearTimeout(timer);
       timer = setTimeout(function(){
         fn.appply(self,arguments);
         timer = null;
       },delay)
     }
   }
   ```

## 简单的Promise

```js
function MyPromise(executor){
  this.value = null;
  this.reason =null;
  this.status = "pending";
  this.onResolvedQueue = [];
  this.onRejectedQueue = [];

  const self = this;
  function resolve(value){
    if(self.status !== 'pending'){
      return;
    }
    self.value = value;
    self.status = 'resolved';
    setTimeout(function(){
      self.onResolvedQueue.forEach(resolved=>resolved(value));
    });
  }
  function reject(reason){
    if(self.status !== 'pending'){
      return;
    }
    self.value = value;
    self.status = 'rejected';
    setTimeout(function(){
      self.onRejectedQueue.forEach(rejected=>rejected(reason));
    });
  }

  executor(resolve,reject);
}

MyPromise.then = function(onResolved,onRejected){
  const self = this;
  if(typeof onResolved !== 'function'){
    onResolved = function(x){return x};
  }
  if(typeof onRejected !== 'function'){
    onRejected = function(e){ throw e }
  }

  if(this.status === 'resolved'){
    onResolved(this.value)
  }else if(this.status === 'rejected'){
    onRejected(this.reason);
  }else if(this.status === 'pending'){
    this.onResolvedQueue.push(onResolved);
    this.onRejectedQueue.push(onRejected);
  }
  return this;
}
```