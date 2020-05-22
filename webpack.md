# webpack记录
## webpack-dev-server好处
devServer会在内存中生成一个打包好的`bundle.js`，存放的路径是根目录下`./bundle.js`,专供开发时使用，打包效率高，修改代码后会自动重新打包以及刷新浏览器，开发人员体验更好。

 ## webpack production模式下的打包优化
 1. tree shaking剔除没有用到的代码
 2. scope hoisting 分析模块关系，推断结果，使webpack打包出来的代码文件更小。
 3. 代码压缩、混淆

## tree shaking
 tree shaking是一个术语，通常用于打包时移除javascript中未引用的代码。其原理是依赖ES6模块系统中的`import`和`export`的静态结构特征

 开发时引入一个模块后，如果只使用其中的一个功能，上线打包时只会把用到的功能打包进bundle，其他没用到的功能都不会打包进来，可是现实最基础的优化

 ## scope hoisting
 scope hoisting的作用是将模块之间的关系进行结果推测，可以将webpack打包出来的代码文件更小、运行的更快。

 scope hoisting的实现原理其实很简单：分析出  模块之间的依赖关系，尽可能的把打散的模块合并到一个函数中去，但前提是不能造成代码冗余。因此只有那些被引用了一次的模块才能被合并

 由于scope hoisting需要分析出模块之间的依赖关系，因此源码必须采用ES6模块化语句，不然它将无法生效。原理和tree shaking一样

## webpack optimization

满足条件然后按组打包

```js
optimization:{
  splitChunks:{
    chunks:'all',//默认为async，异步加载的模块进行代码分割
    minSize:30000,//模块最少大于30KB才拆分
    maxSize:50000,//如果超出maxSize，会进一步进行拆分，可能拆出更多的文件
    minChunks:1,
    cacheGroups:{//缓存组配置，上面配置读取完成后进行拆分，如果需要把多个模块拆分到一个文件就需要缓存
      vendors:{//自定义缓存组名
        test:/[\\/]node_modules[\\/]/,//把用到的node_modules模块打包进到vendors中
        priority:-10,//权重
      },
      default:{
        minChunks:2,//最少引用两次才会被拆分
        priority:-20,
        reuseExistingChunk:true,
      }
    }
  }
}
```

## 利用webpack忽略moment语言包

参数1：表示要忽略的资源路径

参数2：要忽略的资源上下文

`new webpack.IgnorePlugin(/\.\/locale/,/moment/)`

然后手动引入中文语言包，设置语言
```js
import moment from 'moment'
import 'moment/locale/zh-cn'
moment.locale('zh-CN')
```

## 了解webpack打包原理
当webpack处理应用程序时，它会从入口文件开始递归地构建一个依赖关系图，其中包含应用程序需要的每一个模块，然后将所有这些模块打包成一个或多个bundle。

webpack打包步骤：

1. 初始化参数，从配置文件和Shell语句中 读取与合并参数，得出最终参数
2. 用上一步得到的参数初始Compiler对象，加载`webpack.config.js`的配置。加载所有配置的插件，通过执行对象run方法开始执行编译
3. 确定入口，根据配置中的Entry找出所有入口文件。使用node中的fs去读取文件。
4. 编译模块，从入口文件出发，调用所有配置的Loader对模块进行编译，再找出模块依赖的模块，在递归本步骤，直所有入口依赖的文件都经过了本步骤的处理
5. 完成模块编译，在经过第四步使用Loader翻译完所有模块后，得到每个模块被编译后的最终内容以及它们之间的依赖关系
6. 输出文件。根据入口和模块之间的依赖关系，组装成一个个包含多个模块的Chunk，再将每个Chunk转化成一个单独的文件加入输出列表中。
7. 输出完成。在确定好输出内容后，根据配置确定输出的路径和文件名，将文件的内容写入文件系统中。

整个流程分为3个阶段，初始化、编译、输出。而在每个阶段又会发生很多事件，Webpack会将这些事情用tapable进行事件广播出来供Plugin使用。

## 了解webpack的loader原理

loader就像一个翻译员，能将源文件经过转化后输出新的结果，并且一个文件还可以链式地经过多个loader翻译。loader其实就是一个Node.js模块，这个模块需要导出一个函数。函数其中的参数source(源文件代码)是compiler对象传递的。一般使用`loaderUtils`这个库来获取loader中的参数。

## 了解webpack的插件原理 

webpack的插件主要基于Tapable实现的，tapable是webpack项目组的一个内部库，主要抽象了一套事件，这些事件贯穿整个webpack的生命周期。

+ 插件是一个独立的模块
+ 模块对外暴露一个js函数
+ 函数的原型上定义一个注入complier对象的apply方法。apply方法中需要通过complier对象挂载的webpack事件钩子，钩子的回调中能拿到当前编译的compilation对象，如果是异步编译插件的话可以拿到回调callback
+ 完成自定义编译流程并处理compliation对象的内部数据
+ 如果异步编译插件的话，数据梳理完后执行callback回调

## Complier和Compliation

complier对象是webpack的编译器对象，complier对象会在启动webpack的时候被一次性地初始化。complier对象中包含了所有webpack可自定义操作的配置。例如loader的配置、plugin的配置、entry的配置等各种原始webpack配置等

compilation实例继承与complier,compliation对象代表了一次单一的版本webpack构建和生成编译资源的过程。

当运行webpack开发环境中间件时，每当检测到一个文件的变化，一次新的编译将被创建，从而生成一组新的编译资源以及新的compliation对象。一个compliation对象包含了当前模块资源、编译生成资源、变化的文件、以及跟踪依赖的状态信息。编译对象也提供了很多关键点回调供插件做自定义处理时选择使用

根据区别：**Complier代表了整个webpack从启动到关闭的生命周期，而Compliation只代表一次新的编译**

## webpack热更新原理

1. webpack会监控文件，当文件发生变化会重新编译打包。并将打包后的代码通过简单的js对象保存在内存中。
2. webpack-dev-middleware调用webpack暴露的API对代码进行监控，并且告诉webpack，将代码打包到内存中。
3. webpack-dev-server 在浏览器和服务器之间建立一个websocket长连接，当然服务端传递的最主要信息还是新模块的 hash 值，后面的步骤根据这一 hash 值来进行模块热替换
4. HotModuleReplacement.runtime 是客户端 HMR 的中枢，它接收到上一步传递给他的新模块的 hash 值，它会向 服务端发送 Ajax 请求，服务端返回一个 json，该 json 包含了所有要更新的模块的 hash 值，获取到更新列表后，该模块再次通过 jsonp 请求，获取到最新的模块代码
5. 替换已更改的代码内容

## 了解tapable的原理

通过实现发布订阅模式