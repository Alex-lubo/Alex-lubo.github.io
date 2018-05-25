---
title: React学习笔记(1)
date: 2018-05-25 13:03:57
tags:
---

发现React很火了，然后也去了学习一下React？顺便做些笔记，形成此文。

虽然并非专职的前端，项目中用过angular和vue，也了解过静态页面是怎么回事，所以也算是对前端有所了解了。我理解的web前端：一堆配合css的html标签形成漂亮的页面，然后javascript来控制页面的这些标签和css，使页面生动起来。javascript通过浏览器提供的接口，可以定位到指定的页面元素。为了更方便页面元素的操作，后来有了jquery，使得开发人员能更专注与页面逻辑，而不是去写大量代码用来寻找页面元素。随着前端业务的增加，前端开发需要更加灵活和便于扩展，借助大规模软件开发的思想（oop），于是有了angular，vue等等前端框架，这些框架将底层的一些细节（浏览器api，DOM操作）进行了封装，可以方便开发人员对页面功能进行抽象，形成模块（组件），这样通过模块的方式来组织代码，使页面开发的逻辑变得简单，易于维护，而且可以通过模块的复用，提高开发的效率。今天的前端开发，应该没有不用框架的吧？

# React简介
React是facebook开源的前端UI框架。可以用来开发单页面应用和手机应用程序（这个厉害，要一统江湖，千秋万载吗）。具体的可以参考[wiki](https://en.wikipedia.org/wiki/React_(JavaScript_library))。
# 概览
React的核心思想是：封装组件。各个组件自己维护自己的状态和UI，当状态更新，自动重新渲染更新自己的UI。这样一来，可以想想整个页面就全是由组件组合而成的，如果某个DOM在应用中会被操作和改变，那么就把该DOM放在某个组件内，这样你就不需要去操行如何查找定位这个DOM，把关注点放到这个组件的设计上来。简单来说，组件封装，将DOM的操作封装到组件内，与操作它的Javascript紧密的联系在一起，不用在来回去获取和操作DOM了，而且当某个组件发生变化时，只需要重新渲染该组件的UI。提高了页面操作和渲染的效率。

React的组件由DOM视图和state数据组成。state存放数据，它的状态决定视图的状态。当数据改变时，需要调用setState方法来控制组件视图的更新。setState会自动调用render函数，来进行组件视图的重新渲染，如果只是改变state的数据，没有调用setState，那么组件视图也不会出发更新。

```
<!DOCTYPE <!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8" />
  <title>Hello World</title>
  <script src="https://unpkg.com/react@16/umd/react.development.js"></script>
  <script src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
  <script src="https://unpkg.com/babel-standalone@6.15.0/babel.min.js"></script>
</head>
<body>
  <div id="root"></div>
  <script type="text/babel">
  class Main extends React.Component {
      constructor(props) {
        super(props);
        this.state = {
          value:'Please write an essay about your favorite DOM element.',
          fruit: 'coconut'
        };
        this.handleChange = this.handleChange.bind(this);
        this.handleSubmit = this.handleSubmit.bind(this);
        // this.handleSelectChange = this.handleSelectChange.bind(this)
      };
      handleChange(e) {
        console.log(e);
        this.setState({value: e.target.value});
      };
      handleSubmit(e) {
        alert(`a name has submited: ${this.state.value} with favor fruit ${this.state.fruit}`);
        e.preventDefault();
      };
      handleSelectChange = e => {
        this.setState({fruit: e.target.value});
      };
      render() {
        return (
          <form onSubmit={this.handleSubmit}>
            <label value={this.props.name}/>
            <select value={this.state.fruit} onChange={this.handleSelectChange}>
              <option value="grapefruit">Grapefruit</option>
              <option value="lime">Lime</option>
              <option value="coconut">Coconut</option>
              <option value="mango">Mango</option>
            </select>
            <label>
              Name:
              <input type="text" value={this.state.value} onChange={this.handleChange} />  
            </label>
            <div>
              <input type="submit" value="Submit" />
            </div>
            <div>
              <input type="file" />
            </div>
          </form>
        )
      }
    };
    ReactDOM.render(
      <Main name="main"/>,
      document.getElementById('root')
    );
  </script>
</body>
</html>
```
上述代码中，定义了一个React的组件NameForm.页面，数据操作和事件响应均封装在该组件中。

React大体包含下面这些概念：
+ 组件
> render()中定义了组件的视图.数据在this.state和props中.props和state是组件包含的两个核心概念。
> + props 看作是组件的配置属性，在组件内部是不变的，只是在调用这个组件的时候传入不同的属性（比如这里的 name）来定制显示这个组件。
> + 
> 
+ JSX
> 从上面的代码可以看到将 HTML 直接嵌入了 JS 代码里面，这个就是 React 提出的一种叫 JSX 的语法.实际上组件的 HTML 是组成一个组件不可分割的一部分，能够将 HTML 封装起来才是组件的完全体，JSX需要‘编译’后输出JS代码后才能使用。
+ Virtual DOM
> 当组件状态 state 有更改的时候，React 会自动调用组件的 render 方法重新渲染整个组件的 UI。React 实现了一个Virtual DOM，组件 DOM 结构就是映射到这个 Virtual DOM 上，React 在这个 Virtual DOM 上实现了一个 diff 算法，当要重新渲染组件的时候，会通过 diff 寻找到要变更的 DOM 节点，再把这个修改更新到浏览器实际的 DOM 节点上，所以实际上不是真的渲染整个 DOM 树。这个 Virtual DOM 是一个纯粹的 JS 数据结构，所以性能会比原生 DOM 快很多。
+ Data Flow
> “单向数据绑定”是 React 推崇的一种应用架构的方式。有两种实现
> + 官方的Flux
> + Redux


# React组件
组件的创造方法为React.createClass() ——创造一个类，react系统内部设计了一套类系统，利用它来创造react组件。也可以用es6的class来创造组件（见示例），这也是Facebook推荐的写法。
```
class Main extents React.Component {
  constructor(props) {
    super(props);
    this.state = {
      params:'',
      list:[]
    };
  }
}
```
es6的class类可以其实只是构造函数的一个语法糖。定义组件式需要继承React.Component的属性和方法，从而使定义的组件有了React的生命周期函数。class里面有构造函数，在实例化对象时，首先调用super创造父类的实例对象，然后用子类的构造进行修改。当使用组件<Main />时，其实就是在对Main进行实例化，只不过React对实例化过程进行了封装，使之看起来像是一个tag。
需要**注意**：
+ 组件（类）名首字母必须要大写
+ 类选择器要用className来代替，因为class已经被用来定义类了
+ 组件内部默认使用严格模式

当然，也可直接用函数来写一个组件。
```
function Welcome(props) {
  return <h1>Hello world</h1>;
}
```
# React组件的生命周期

![](https://github.com/Alex-lubo/CodeSnippets/blob/master/graphics/React%20lifecycle.png)

如图，React组件的生命周期包括三个阶段：初始化阶段、运行中阶段和销毁阶段。在不同的生命周期里，会依次触发不同的钩子函数。
+ getDefaultProps
> 只在组件创建时调用一次并缓存返回的对象（即在 React.createClass 之后就会调用）.因为这个方法在实例初始化之前调用，所以在这个方法里面不能依赖 this 获取到组件的实例。在组件装载之后，这个方法缓存的结果会用来保证访问 this.props 的属性时，即使没有在父组件中传入（在这个组件的 JSX 属性里设置），也总是有值的。es6中可以使用dufaultProps来设置。
> ```
> class Hello extents React.Component {
>   constructor(props) {
>     super(props);
>     this.state = {};
>   };
>   getDefaultProps() {
>     return {
>       name: 'alex'
>     } 
>   };
> }
> // or
> Hello.defaultProps = {name: 'abc'}
> ```
+ getInitialState
> 初始化 this.state 的值，只在组件装载之前调用一次。如果是使用 ES6 的语法，可以直接在构造函数中初始化状态；
+ componentWillMount
> 整个生命周期只调用一次，只在组件视图渲染前调用。在渲染之前最后修改state的机会。可以做开启定时器、向服务器发请求等操作。
+ render
> 组装生成这个组件的 HTML 结构（使用原生 HTML 标签或者子组件），创建虚拟DOM。进行diff算法，更新dom树都在此进行。此时就不能更改state了。也可以返回 null 或者 false；
+ componentDidMount
> 该方法被调用时，已经渲染出真实的 DOM，就是虚拟DOM已经插入文档之中了。可以通过this.getDOMNode()获取和操作dom节点，只调用一次。如果要获取真实DOM的节点，需要用到ref属性。ref是真实DOM的属性。
+ componentWillReceiveProps
> 组件的 props 属性可以通过父组件来更改，这时，componentWillReceiveProps 将来被调用，可以在这个方法里更新 state,以触发 render 方法重新渲染组件。
> ```
> componentWillReceiveProps(nextProps) {
>    if(nextProps.checked !== undefined){
>        this.setState({
>           checked: nextProps.checked
>       })
>    }
> }
> ```
+ shouldComponentUpdate
> 当组件接收到新属性，或者组件的状态发生改变时触发。组件首次渲染时并不会触发.可以设置在此对比前后两个props和state是否相同，如果相同则返回false阻止更新，因为相同的属性状态一定会生成相同的dom树，这样就不需要创造新的dom树和旧的dom树进行diff算法对比，节省大量性能，尤其是在dom结构复杂的时候。不过调用this.forceUpdate会跳过此步骤。
> ```
> shouldComponentUpdate(nextProps, nextState) {
>   return this.state.checked === nextState.checked;
> }
> ```

+ componentWillUpdate
> 和 componentWillMount 类似，在组件接收到了新的 props 或者 state 即将进行重新渲染前，componentWillUpdate(object nextProps, object nextState) 会被调用，注意不要在此方面里再去更新 props 或者 state。
+ componentDidUpdate
> 和 componentDidMount 类似，在组件重新被渲染之后，componentDidUpdate(object prevProps, object prevState) 会被调用。可以在这里访问并修改 DOM。
+ componentWillUnmount
> React使用完一个组件，这个组件必须从 DOM 中卸载后被销毁，此时 componentWillUnmout 会被执行，完成所有的清理和销毁工作，在 componentDidMount 中添加的任务都需要再该方法中撤销，如创建的定时器或事件监听器。
> 当组件再次装载时，组件周期从头开始。

# 组件间通信
父子组件之间很简单，可以通过props属性来传递。在父组件设置props，然后子组件通过props可以访问到父组件传递过来的数据或方法，从而建立其父子组件通信的桥梁。如例子中<Main name="main">其实就是通过props将name属性传递到子组件的。

对于非父子组件间的通信，使用全局事件 Pub/Sub 模式，在 componentDidMount 里面订阅事件，在 componentWillUnmount 里面取消订阅，当收到事件触发的时候调用 setState 更新 UI。
这种模式在复杂的系统里面可能会变得难以维护，所以看个人权衡是否将组件封装到大的组件，甚至整个页面或者应用就封装到一个组件。

# Data Flow
React 标榜自己是 MVC 里面 V 的部分，那么 Flux 就相当于添加 M 和 C 的部分。
Flux 是 Facebook 使用的一套前端应用的架构模式。
一个 Flux 应用主要包含四个部分：
+ the dispatcher
> 处理动作分发，维护 Store 之间的依赖关系
+ the stores
> 数据和逻辑部分
+ the views
> React 组件，这一层可以看作 controller-views，作为视图同时响应用户交互
+ the actions
> 提供给 dispatcher 传递数据给 store

Flux 的核心“单向数据流“怎么运作的：
```
Action -> Dispatcher -> Store -> View
```
整个流程如下：
+ 首先要有 action，通过定义一些 action creator 方法根据需要创建 Action 提供给 dispatcher
+ View 层通过用户交互（比如 onClick）会触发 Action
+ Dispatcher 会分发触发的 Action 给所有注册的 Store 的回调函数
+ Store 回调函数根据接收的 Action 更新自身数据之后会触发一个 change 事件通知 View 数据更改了
+ View 会监听这个 change 事件，拿到对应的新数据并调用 setState 更新组件 UI

所有的状态都由 Store 来维护，通过 Action 传递数据，构成了如上所述的单向数据流循环，所以应用中的各部分分工就相当明确，高度解耦了。