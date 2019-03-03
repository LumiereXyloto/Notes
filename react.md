### 主要概念

##### JSX

+ 编译后JSX转化为js对象
+ 在JSX中使用js表达式要包含在大括号中
+ 推荐在JSX代码外扩上一个小括号，防止分号自动插入的bug
+ 用“”定义字符串为值的属性，用{}定义表达式为值的属性
+ 如果 JSX 标签是闭合式的，那么你需要在结尾处用 `/>`
+ ReactDom使用小驼峰命名属性
+ Babel 转译器会把 JSX 转换成一个名为 `React.createElement()` 的方法调用



##### 元素渲染

+ React 元素都是immutable 不可变的。当元素被创建之后，你是无法改变其内容或属性的

+ 实际生产开发中，大多数React应用只会调用一次 ReactDOM.render()



##### 组件&Props

+ 定义组件

  1. ```javascript
     function Welcome(props) {
       return <h1>Hello, {props.name}</h1>;
     }
     ```

  2. ```javascript
     class Welcome extends React.Component {
       render() {
         return <h1>Hello, {this.props.name}</h1>;
       }
     }
     ```

+ 当React遇到的元素是用户自定义的组件，它会将JSX属性作为单个对象传递给该组件，这个对象称之为“props”。 props是只读的

+ [“纯函数”](https://en.wikipedia.org/wiki/Pure_function)，它没有改变它自己的输入值，当传入的值相同时，总是会返回相同的结果。 

+ 所有的React组件必须像纯函数那样使用它们的props

+ 局部状态，生命周期钩子只能用于类



##### State&生命周期

+ 将函数转化为类

  1. 创建一个名称扩展为 `React.Component` 的[ES6 类](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes) 
  2. 创建一个叫做`render()`的空方法 
  3. 将函数体移动到 `render()` 方法中 
  4. 在 `render()` 方法中，使用 `this.props` 替换 `props` 
  5. 删除剩余的空函数声明 

+ 类组件应始终使用`props`调用基础构造函数 

+ setState()

  ```javascript
  this.setState({comment: 'Hello'});
  ```

+ 不要直接用this.state更改状态

+ 构造函数是唯一可以初始化this.state的地方

+ 状态的更新可能是异步的，react可以将多个setState()合并提高性能，所以当有多个状态需要在一个setState()里面更新的时候，应该使用第二种形式的setState()

  该函数将接收先前的状态作为第一个参数，将此次更新被应用时的props做为第二个参数 

  ```javascript
  this.setState((prevState, props) => ({
    counter: prevState.counter + props.increment
  }));
  ```



##### 事件处理

+ 在react中不能用返回false阻止默认行为，必须明确的使用preventDefault

  ```javascript
  function ActionLink() {
    function handleClick(e) {
      e.preventDefault();
      console.log('The link was clicked.');
    }
  
    return (
      <a href="#" onClick={handleClick}>
        Click me
      </a>
    );
  }
  ```

+ 谨慎对待 JSX 回调函数中的 `this`，类的方法默认是不会[绑定](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_objects/Function/bind)`this` 的 。

+ 事件对象？？？和this



##### 条件渲染

+ 可以使用变量来储存元素。它可以帮助你有条件的渲染组件的一部分，而输出的其他部分不会更改。

+ `true && expression`总是返回 `expression`，而 `false && expression` 总是返回 `false`。 

  因此，如果条件是 `true`，`&&` 右侧的元素就会被渲染，如果是 `false`，React 会忽略并跳过它。 

+  三目运算符 condition ? true : false

+ 如果条件过于复杂，应该考虑提取组件

+ 让render()方法返回null而不是渲染结果就可以实现隐藏组件

  ```javascript
  function WarningBanner(props) {
    if (!props.warn) {
      return null;
    }
  
    return (
      <div className="warning">
        Warning!
      </div>
    );
  }
  ```



##### 列表&Keys

+ Keys可以在DOM中的某些元素被增加或删除的时候帮助React识别哪些元素发生了变化 

+ 如果项目列表的顺序可能有变化，不建议用index当key，那样会导致性能的负面影响，还可能引起状态问题

+ 如果你提取出一个`ListItem`组件，你应该把key保存在数组中的这个`<ListItem />`元素上，而不是放在`ListItem`组件中的`<li>`元素上 

+ key在兄弟间必须是唯一的，但不需要是全局唯一的，所以在两个不同的数组可以使用相同的key

+ 如果组件需要使用和key相同的值，应该用其他属性名显示传递，因为key不会传递给组件

  ```javascript
  const content = posts.map((post) =>
    <Post
      key={post.id}
      id={post.id}
      title={post.title} />
  );
  ```

+ 如果map()嵌套了太多层级，那可能是提取组件的一个好时机。



##### 表单

+ 受控组件

+ 使用计算属性名更新状态

  ```javascript
  this.setState({
    [name]: value
  });
  ```

  

##### 状态提升

+ 当几个组件需要共用状态的时候，最好将共享状态提升至最近的父组件中进行管理。



##### 组合&继承

+ 如果要在组件之间复用 UI 无关的功能，建议将其提取到单独的 JavaScript 模块中 
+ 组件可以接受任意元素，包括基本数据类型、React 元素或函数。 



##### React理念

+ 找出state

  1. 它是通过 props 从父级传来的吗？如果是，他可能不是 state。
  2. 它随着时间推移不变吗？如果是，它可能不是 state。
  3. 你能够根据组件中任何其他的 state 或 props 把它计算出来吗？如果是，它不是 state。

+ state该放在哪个组件

  - 确定每一个需要这个 state 来渲染的组件。
  - 找到一个公共所有者组件(一个在层级上高于所有其他需要这个 state 的组件的组件)
  - 这个公共所有者组件或另一个层级更高的组件应该拥有这个 state。
  - 如果你没有找到可以拥有这个 state 的组件，创建一个仅用来保存状态的组件并把它加入比这个公共所有者组件层级更高的地方。

  





