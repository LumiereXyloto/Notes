+ beforeCreate 实例创建之前 在这个阶段我们写的代码还没有被运行
+ created 实例创建之后  在这个阶段我们写的代码已经运行了但是还没有将 HTML 渲染到页面
+ mounted 挂载之后 在这个阶段 html 渲染到页面了，可以取到 dom 节点
+ beforeUpdate 数据更新前 在我们需要重新渲染 html 前调用 类似执行 warp.innerHTML = html; 之前
+ updated 数据更新后  在重新渲染 HTML 后调用
+ destroyed 实例销毁后调用  将我们写的代码丢弃掉后调用
+ errorCaptured 当捕获一个来自子孙组件的错误时被调用 2.5.0+ 新增
