### JS模块化

##### AMD规范

+ Async Module Definition
+ 使用define定义模块
+ 使用require加载模块
+ Require.js
+ 依赖前置，提前执行(模块尽可能前置)

##### CMD规范

+ Common Module Definition
+ 一个文件为一个模块
+ 使用define定义模块
+ 使用require加载模块
+ Sea.js
+ 尽可能懒执行(和AMD最显著的区别)

##### UMD规范

+ Universal Module Definition通用解决方案

+ 三个步骤

+ + 判断是否支持AMD

  + 判断是否支持Common.js
  + 如果都没有 使用全局变量

##### ESM规范

+ EcmaScript Module
+ 一个文件一个模块
+ import/export
+ import module from lib会使用模块暴露的接口，只import lib不会使用暴露的接口
+ export/export default



---

webpack 支持AMD(RequireJS)，ES Modules(推荐的)和CommonJS

---



### CSS模块化

##### CSS 设计模式

样式类和容器分离，多层级样式，属性编码样式设计，块/块中元素/元素状态三部分分离设计等，有很多种设计模式





### Webpack基础
![webpack打包原理](.\images\webpack.PNG)

---

##### 核心概念

1. Entry 代码的入口，找到依赖的模块，间接依赖的模块；打包的入口；单个或多个入口

2. Output 打包生成的文件(bundle)；一个或多个；自定义规则
3. Loaders 处理文件；转化为模块；
4. Plugins 参与打包整个过程；打包优化和压缩；配置编译时的变量；非常灵活
5. Chunk 代码块；bundle；Module



### 使用webpack

##### webpack命令

+ 打包JS

  webpack entry<entry> output

  webpack --config webpack.conf.js

+ 运行配置文件

  webpack （默认运行webpack.config.js）

  webpack --config webpack.conf.js（自定义配置文件名称后的运行方式）



### Babel配置

##### babel preset-env

##### babel polyfill

+ 全局垫片，为应用准备，让api在全局环境调用，把没有的方法放到全局让你使用
+ npm install babel-polyfill --save
+ import 'babel-polyfill'

##### babel runtime transform
+ 局部垫片，为开发框架准备 (比如使用vue，element这些会用到es6的框架的时候可以考虑使用，这样不会污染全局变量)
+ npm install babel-plugin-transform-runtime --save-dev
+ npm install babel-runtime --save
+ 在根目录新建.babelrc文件然后在里面进行targets，presets，plugin等设置

##### babel-loader配置

![](.\images\babel_loader.PNG)



***Note: options中的内容也可以在根目录创建名字为.babelrc的JSON文件，然后放在里面。***

![](.\images\\babelrc.PNG)





### Typescript配置

##### typescript-loader

+ 安装 npm install typescript ts-loader --save-dev    （官方）

  ​	 npm install typescript awesome-typescript-loader --save-dev    （第三方）

+ 配置 tsconfig.json

  ​	 webpack.config.js

##### tsconfig
+ 配置选项

  http://www.typescriptlang.org/docs/handbook/compiler-options.html

+ 常用选项

  + compilerOptions 告诉ts编译器常用的选项

  + include 给出文件路径

  + exclude 不处理，排除在规则之外
##### 声明文件

+ 显示ts编译反馈，传参错误，类型错误等提示（未使用类型声明文件是没有报错提示的）

+ npm install @types/lodash

+ npm install @types/vue

+ 也可以全局安装typings   npm install typings -g，然后typings install lodash去装相关类库的声明

+ 安装声明文件以后在tsconfig.json的compilerOptions里添加typeRoots，它是一个数组，元素为声明文件所在的目录，例如：

  ![](.\images\typeRoots.PNG)





### 提取共用代码

##### 作用

+ 减少代码冗余
+ 提高加载速度



##### CommonChunkPlugin配置

+ //optimization与entry/plugins同级
  optimization: {
          splitChunks: {
              cacheGroups: {
                  commons: {
                      name: "commons",
                      chunks: "initial",
                      minChunks: 2
                  }
              }
          }
      }

  1. commons里面的name就是生成的共享模块bundle的名字
  2. chunks 有三个可选值，”initial”, “async” 和 “all”. 分别对应优化时只选择初始的chunks，所需要的chunks 还是所有chunks 
  3. minChunks 是split前，有共享模块的chunks的最小数目 ，默认值是1 




