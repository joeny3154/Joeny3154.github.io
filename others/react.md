


# this.setState

this.setState

可以将多个setState() 调用合并成一个调用来提高性能

可以将多个setState() 调用合并成一个调用来提高性能

要修复它，请使用第二种形式的 setState() 来接受一个函数而不是一个对象。 该函数将接收先前的状态作为第一个参数，将此次更新被应用时的props做为第二个参数：


this.setState((prevState, props) => ({
  counter: prevState.counter + props.increment
}));


# 事件

- 区别
<button onclick="activateLasers()">
  Activate Lasers
</button>


例如，传统的 HTML：

<button onclick="activateLasers()">
  Activate Lasers
</button>

React 中稍稍有点不同：

<button onClick={activateLasers}>
  Activate Lasers
</button>

在 React 中另一个不同是你不能使用返回 false 的方式阻止默认行为。你必须明确的使用 preventDefault。

- this绑定

`handleClick = () => {console.log('this is:', this);}` => `<button onClick={this.handleClick}>Click me</button>`

或者

`<button onClick={(e) => this.handleClick(e)}>Click me</button>`但每次渲染都会重新创建不同的回调函数，通常建议在构造函数中绑定或使用属性初始化器语法来避免这类性能问题。


- 传参：

方式1：
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>

方式2：
通过 bind 方式向监听函数传参，在类组件中定义的监听函数，事件对象 e 要排在所传递参数的后面

<a href="https://reactjs.org" onClick={this.preventPop.bind(this,this.state.name)}>Click</a>

``` js
//事件对象e要放在最后
preventPop(name, e){
  e.preventDefault();
  alert(name);
}
```

# 阻止组件渲染

组件的 render 方法返回 null 并不会影响该组件生命周期方法的回调。例如，componentWillUpdate 和 componentDidUpdate 依然可以被调用。


# 表单

受控组件：其值由React控制的输入表单元素称为“受控组件”

非受控组件： 如file input 标签，其 value 属性是只读的， 所以它是 React 中的一个非受控组件

# 组合：children

使用 children 属性将子元素直接传递到输出

``` jsx
<div className={'FancyBorder FancyBorder-' + props.color}>{props.children}</div>

<FancyBorder color="blue">
  <h1 className="Dialog-title">
    Welcome
  </h1>
  <p className="Dialog-message">
    Thank you for visiting our spacecraft!
  </p>
</FancyBorder>
```

# 性能优化：避免重复渲染

使用`immutable.js`创建不可突变对象，不可突变的数据结构让我们在实现shouldComponentUpdate时，帮助我们轻松的追踪对象变化。这通常可以提供一个不错的性能提升。

# jsx深入

点表示法：`<MyComponents.DatePicker color="blue" />;`

在运行时选择类型: 

``` js
function Story(props) {
  // 错误！JSX 标签名不能为一个表达式。
  return <components[props.storyType] story={props.story} />;
}

function Story(props) {
  // 正确！JSX 标签名可以为大写开头的变量。
  const SpecificStory = components[props.storyType];
  return <SpecificStory story={props.story} />;
}
```

默认为 True: `<MyTextBox autocomplete />`

扩展属性: 

``` js
function App2() {
  const props = {firstName: 'Ben', lastName: 'Hector'};
  return <Greeting {...props} />;
}
```


JavaScript 中的一些 “falsy” 值(比如数字0)，它们依然会被渲染

``` jsx
{props.messages.length && <MessageList messages={props.messages} />}

{props.messages.length > 0 && <MessageList messages={props.messages} />}
```

相反，如果你想让类似 false、true、null 或 undefined 出现在输出中，你必须先把它转换成字符串 :

``` js
<div>
  My JavaScript variable is {String(myVariable)}.
</div>
```

# ref & dom

eact 支持给任意组件添加特殊属性。ref 属性接受一个回调函数，它在组件被加载或卸载时会立即执行。

- 为 DOM 元素添加 Ref

- 为类组件添加 Ref: 

``` jsx
componentDidMount() {
  this.textInput.focusTextInput();
}
render() {
  return <CustomTextInput ref={(input) => { this.textInput = input; }} />
}
```

- 你不能在函数式组件上使用 ref 属性, 因为它们没有实例：

- 但可以在函数式组件内部使用 ref，只要它指向一个 DOM 元素或者 class 组件

###  对父组件暴露 DOM 节点

```jsx
function CustomTextInput(props) {
  return (
    <div>
      <input ref={props.inputRef} />
    </div>
  );
}

class Parent extends React.Component {
  render() {
    return (
      <CustomTextInput
        inputRef={el => this.inputElement = el}
      />
    );
  }
}
```

# 不受控组件

要编写一个非受控组件，而非为每个状态更新编写事件处理程序，你可以 **使用 ref 从 DOM 获取表单值**。

``` jsx
handleSubmit(event) {
  alert('A name was submitted: ' + this.input.value);
  event.preventDefault();
}

<input type="text" ref={(input) => this.input = input} />
```

默认值： defaultValue 属性而不是 value

<input defaultValue="Bob" type="text" ref={(input) => this.input = input} />

同样，<input type="checkbox"> 和 <input type="radio"> 支持 defaultChecked，<select> 和 <textarea> 支持 defaultValue.

# 代码分割


``` js
import("./math").then(math => {
  console.log(math.add(16, 26));
});
```


``` js
import Loadable from 'react-loadable';

const LoadableOtherComponent = Loadable({
  loader: () => import('./OtherComponent'),
  // 预加载
  loading: () => <div>Loading...</div>,
});
```

# redux

