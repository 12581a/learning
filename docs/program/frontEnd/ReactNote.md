# React基础

## 虚拟dom的两种创建方式

```javascript
<script type="test/babel">
    const VDOM = <h1 id="title">Hello,React</h1>;
    ReactDOM.render(VDOM, document.getElementById("app"));
</script>

<script type="test/javaScript">
    const VDOM = React.createElement('h1',{id:'title'},'Hello,React')
    ReactDOM.render(VDOM, document.getElementById("app"));
</script>
```

## jsx语法规则

定义虚拟DOM时，不要写引号。
标签中混入**JS表达式**时要用{}。
样式的类名指定不要用class，要用className。
内联样式，要用style={{key:value}}的形式去写。
只有一个根标签
标签必须闭合
标签首字母
(1).若小写字母开头，则将该标签转为html中同名元素，若html中无该标签对应的同名元素，则报错。
(2).若大写字母开头，react就去渲染对应的组件，若组件没有定义，则报错。	

```javascript
<style>
	.title{}
</style>
<script type="test/babel">
    let myId = "title";
	let context = "";
    const VDOM = (
        <div>
        	<h1 id = {myId} className='title' style={{ color:white }}>{context}</h1>
        </div>
    )
    ReactDOM.render(VDOM, document.getElementById("app"));
</script>
```

**jsx练习**

```javascript
		//模拟一些数据
		const data = ['Angular','React','Vue']
		//1.创建虚拟DOM
		const VDOM = (
			<div>
				<h1>前端js框架列表</h1>
				<ul>
					{
						data.map((item,index)=>{
							return <li key={index}>{item}</li>
						})
					}
				</ul>
			</div>
		)
```

## 组件

**函数式组件**

```javascript
//1.创建函数式组件
//React解析组件标签，找到了MyComponent组件。
//发现组件是使用函数定义的，随后调用该函数，将返回的虚拟DOM转为真实DOM，随后呈现在页面中。
function MyComponent(){
	console.log(this); //此处的this是undefined，因为babel编译后开启了严格模式
	return <h2>我是用函数定义的组件(适用于【简单组件】的定义)</h2>
}
ReactDOM.render(<MyComponent/>,document.getElementById('test'))

```

**类式组件**

```javascript
<script type="text/babel">
		//1.创建类式组件
		class MyComponent extends React.Component {
			render(){
				//render是放在哪里的？—— MyComponent的原型对象上，供实例使用。
				//render中的this是谁？—— MyComponent的实例对象 <=> MyComponent组件实例对象。
				console.log('render中的this:',this);
				return <h2>我是用类定义的组件(适用于【复杂组件】的定义)</h2>
			}
		}
		//2.渲染组件到页面
		ReactDOM.render(<MyComponent/>,document.getElementById('test'))
</script>
```

### 组件之间通信

1. `使用props`

2. 消息订阅与发布机制

## 组件实例三大属性

### state

组件被称为"状态机", 通过更新组件的state来更新对应的页面显示(重新渲染组件)

```javascript
class MyComponent extends React.Component{
        state = {flag:true}
        render() {
            const {flag} = this.state;
            return <h1 onClick = {this.change}>Hello,state | {flag ? 'YES' : 'NO'}</h1>    
        }
        change = ()=>{
            const flag = this.state.flag;
            this.setState({flag:!flag})
        }
}
```

### props

定义组件时可以传给组件参数，在组件中可以利用`props`属性来接收。通过标签属性从组件外向组件内传递变化的数据，组件内部不要修改props数据  

```javascript
class MyComponent extends React.Component{
        render() {
            const {name, age} = this.props;
            return (
                <ul>
                    <li>{name}</li>
                    <li>{age}</li>
                </ul>
            ) 
        }
    }
ReactDOM.render(<MyComponent name="zhangsan" age="18"/>, document.getElementById("app"));
```

在函数式组件之中，可以利用传过来的`props`属性来接收参数。

```javascript
function FunComponent(props){
	const {name, age} = props;
    return (
		<ul>
			<li>{name}</li>
            <li>{age}</li>
        </ul>
    ) 
}
```

### ref

ref写法有三种，最简单的一种是在要获取的元素上绑定`ref`属性，然后就可以在`this.refs`上获取

```javascript
// 伪代码
<input type="text" ref="input1" placeholder="点击按钮提示数据"/>
<button onClick={this.getData}>点击</button>
getData = ()=>{
	const {input1} = this.refs;
	alert(input1.value);
}
```

第二种是我们经常使用的方法，采用回调函数的写法

```javascript
<input type="text" ref={currentNode => this.input1 = currentNode}  placeholder="点击按钮提示数据"/>
<button onClick={this.getData}>点击</button>
getData = ()=>{
	const {input1} = this;
	alert(input1.value);
}
```

第三种利用`createRef`来创建

```javascript
myRef1 = React.createRef()
<input type="text" ref={this.myRef1}/>
<button onClick={this.click1}>点击1</button>
click1 = ()=>{
	alert(this.myRef1.current.value)
}
```

## 高阶函数

如果一个函数符合下面2个规范中的任何一个，那该函数就是高阶函数。
(1) 若A函数，接收的参数是一个函数，那么A就可以称之为高阶函数。
(2) 若A函数，调用的返回值依然是一个函数，那么A就可以称之为高阶函数。
常见的高阶函数有：`Promise`、`setTimeout`、`arr.map()`等等。

函数的柯里化：通过函数调用继续返回函数的方式，实现多次接收参数最后统一处理的函数编码形式。

```javascript
function sum(a){
	return(b) =>{
		return(c) =>{
			return a+b+c;
		}
	} 
}
```

表单提交案例

```javascript
<input type="text" name="username" onChange={(event)=>{this.saveData("username",event)}}/>
<input type="password" name="password"  onChange={(event)=>{this.saveData("password",event)}}/>
saveData(data,event){
	this.setState({[data]:event.target.value})
}
```

## 生命周期

生命周期的三个阶段

> 初始化阶段: 由ReactDOM.render()触发---初次渲染

1.	constructor()
2.	**getDerivedStateFromProps** 
3.	render()
4. componentDidMount()

>  更新阶段: 由组件内部this.setSate()或父组件重新render触发

1. **getDerivedStateFromProps**
2. shouldComponentUpdate()
3. render()
4. **getSnapshotBeforeUpdate**
5. componentDidUpdate()

> 卸载组件: 由ReactDOM.unmountComponentAtNode()触发

1. componentWillUnmount()

重要的勾子

**render**：初始化渲染或更新渲染调用

**componentDidMount**：开启监听, 发送ajax请求

**componentWillUnmount**：做一些收尾工作, 如: 清理定时器

## key的作用

react/vue中的key有什么作用？（key的内部原理是什么？）

为什么遍历列表时，key最好不要用index?

1. 虚拟DOM中key的作用：
   简单的说: key是虚拟DOM对象的标识, 在更新显示时key起着极其重要的作用。

   详细的说: 当状态中的数据发生变化时，react会根据【新数据】生成【新的虚拟DOM】, 随后React进行【新虚拟DOM】与【旧虚拟DOM】的diff比较，比较规则如下：

   a. 旧虚拟DOM中找到了与新虚拟DOM相同的key：

   若虚拟DOM中内容没变, 直接使用之前的真实DOM
   若虚拟DOM中内容变了, 则生成新的真实DOM，随后替换掉页面中之前的真实DOM

   b. 旧虚拟DOM中未找到与新虚拟DOM相同的key，根据数据创建新的真实DOM，随后渲染到到页面

2. 用index作为key可能会引发的问题

   若对数据进行：逆序添加、逆序删除等破坏顺序操作:会产生没有必要的真实DOM更新 ==> 界面效果没问题, 但效率低

   如果结构中还包含输入类的DOM：会产生错误DOM更新 ==> 界面有问题

   注意！如果不存在对数据的逆序添加、逆序删除等破坏顺序操作，仅用于渲染列表用于展示，使用index作为key是没有问题的

3. 开发中如何选择key？

   最好使用每条数据的唯一标识作为key, 比如id、手机号、身份证号、学号等唯一值

   如果确定只是简单的展示数据，用index也是可以的

## react脚手架

第一步，全局安装：`npm i -g create-react-app`
第二步，切换到想创项目的目录，使用命令：`create-react-app hello-react`
第三步，进入项目文件夹：`cd hello-react`
第四步，启动项目：`npm start`



# React路由

## 路由的基本使用

旧版本中的使用

```javascript
import { BrowserRouter as Router, Route, Link } from 'react-router-dom'
const First = () => <p>页面一的内容</p>
// 3 使用Router组件包裹整个应用
const App = () => (
  <Router>
    <div>
      <h1>React路由基础</h1>
      {/* 4 指定路由入口 */}
      <Link to="/first">页面一</Link>
      {/* 5 指定路由出口 */}
      <Route path="/first" component={First} />
    </div>
  </Router>
)
```

新版本中的使用

```javascript
import React from 'react'
import ReactDOM from 'react-dom'
/* 
  react-router-dom 的基本使用：
  1 安装： npm i react-router-dom
*/
// 2 导入组件：
import { BrowserRouter as Router, Route, Link, Routes } from 'react-router-dom'
const First = () => <p>页面一的内容</p>
const Home = () => <p>我是首页</p>
// 3 使用Router组件包裹整个应用
const App = () => (
  <Router>
    <div>
      <h1>React路由基础</h1>
      {/* 4 指定路由的入口 */}
      <Link to="/first">页面一</Link>
      {/* 5 指定路由出口 */}
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/first" element={<First />} />
      </Routes>
    </div>
  </Router>
)
```

**switch的使用**：通常情况下，path和component是对应的关系，switch可以实现单一匹配。

**路由的模糊匹配**：默认开启的是模糊匹配，即`path=/home`，在Link中`to=/home/page`可以匹配的上。可以开启严格匹配。

**Redirect**：`<Redirect to="/">`，一般卸载路由组件的最下方，当所有路由都无法匹配时，跳转到Redirect指定的路由。

```javascript
<div className="show">
	<Switch>
		<Route path="/about" component={About}/>
		<Route path="/home" component={Home}/>	
		<Redirect to="/home" />
	</Switch>
</div>
```

**嵌套路由**

## 路由传参

**params参数**：在定义路由链接时`<Link to='/home/page/1001'}></Link>`，定义路由出口时`<Route path="/home/page/:id" component={}/>`

在组件中就可以使用`this.props.match.params`接收数据

**search参数**：在定义路由链接时,`<Link to='/home/page/?name=zhangsan&age=18'></Link>`，定义路由出口时，无需声明，正常注册即可，`<Route path="/home/page" component={}/>`，在组件之中可以使用`this.props.location.search`接收数据

**state参数**：在定义路由链接时,`<Link to={{path='/home/page',state:{name:"zhangsan",age:18}}}></Link>`，定义路由出口时，正常注册即可，`<Route path="/home/page" component={}/>`，在组件之中可以使用`this.props.location.state`接收数据

## 编程式导航

使用`this.props.history`上面的方法

> `withRouter`可以加工一般组件，让一般组件具备路由组件所特有的API。
>
> 用法：export default withRouter(Header)

# redux

redux是一个专门用于做状态管理的JS库，它可以用在react, angular, vue等项目中, 但基本与react配合使用。

作用：可以集中式管理react应用中多个组件共享的状态。  

什么情况下需要使用redux

1. 某个组件的状态，需要让其他组件可以随时拿到（共享）。
2. 一个组件需要改变另一个组件的状态（通信）。
3. 总体原则：能不用就不用, 如果不用比较吃力才考虑使用。

redux的三个核心概念：action、reducer、store

**react-redux的使用**：

UI组件：不能使用任何redux的API，只是负责页面的呈现与交互。

容器组件：负责和redux通信，将结果交给UI组件。

如何创建一个UI组件，使用react-redux的connection函数。

**react-redux的优化**：

1. 容器组件与UI组件整合成一个文件。
2. 无需给容器组件传递store，给APP包裹一个<Provider store = {store}>即可。
3. 使用了react-redux后不用自己检测redux的状态了，容器组件自己可以完成这个工作。
4. .mapDispatchToProps可以简写为一个对象。

# 其他

1. 对象式的setState：`this.setState({num:1},()=>{})`。callback是可选的回调函数，它在状态更新完毕后才被调用。

   函数式的setState：`this.setState((state, props)=>{num:state.num+1},()=>{})`

2. lazyLoad懒加载

```javascript
const About = lazy(()=> import('./About'))
<Suspense fallback={}>
    <Route path='/about' component={About}/>
</Suspense> 
```

3. Hooks

Hook是在React16.8中增加的新特性，可以让你在函数组件中使用state以及其他的React特性

State Hook，让函数组件也可以有state状态，并进行组件的读写操作。参数：第一次初始化指定的值在内部作缓存。返回值是包含两个元素的数组，第一个为内部当前状态值，第二个为更新状态值的函数。

```javascript
function Add(){
    const [sum, Addtion] = React.useState(0);
    //function add(){Addtion(sum+1)}
    function add(Addtion(sum => sum + 1))
    return (<div><button onClick = {add}></button></div>)
}
```

Effect Hook可以让你在函数式组件中执行副作用操作，类似于模拟类组件中的生命周期钩子。包括发送ajax请求、设置订阅、启动定时器

可以把useEffect Hook看做是三个函数的组合。componentDidMount/componentDidUpdate/componentWillUnmount

```javascript
function Add(){
    React.useEffect(()=>{
        // 在此可以执行任何副作用的操作
        let timer = setInterval(()=>{
            Addtion(sum => sum + 1)
        },1000)
        // 在组件卸载之前执行
        return ()=>{clearInterval(timer)}
	},[sum])//如果指定的是[]，回调函数只会在第一次render()后执行
}
```

Ref Hook，可以在函数组件之中存储、查找组件内的标签或任意其他数据

```javascript
const myRef = React.useRef()
<input type="text" ref={myRef}>
```

4. Fragment

可以不用再组件中写div标签

```javascript
<Fragment></Fragment>
```

5. Context

一种组件之间通信的方式，常用于祖祖件与后代组件的通信。

```javascript
// 创建context对象
const MyContext = React.createContext()
const {Provider,Consumer} = MyContext
// B组件及其子组件可以获取value
<Provider value={value}>
    <B/>
<Provider>
// 后代读取组件
// 声明接收context，仅适用于类组件
static contextType = MyContext
this.context
// 函数组件与类组件都可以
<Consumer>
 	{
    	value =>{return value}
	}   
<Consumer/>    
```

6. 组件优化

Component有两个问题

> 只要执行setState()，即使不改变状态数据，组件也会重新render
>
> 只当前组件重新render，就会自动重新render子组件，效率低

只有当组件的props或state数据发生变化时才重新render

> 原因：Component中的shouldComponentUpdate()总是返回为true

解决

* 重写`shouldComponentUpdate`方法

```javascript
shouldComponentUpdate(nextProps, nextState){
    // 判断逻辑
    return true;
}
```

* 使用PureComponent

> 只是进行state和props数据的浅比较，如果只是数据对象内部数据变了，返回false
>
> 不要直接修改state数据，而是要产生新数据

7. renderProps

vue中，使用插槽技术，也就是通过组件标签体传入结构<A><B/></A>

在React中，使用children props：通过组件标签体传入结构，使用render props：通过组件标签属性传入结构，一般用render函数属性

```javascript
// 当组件的父子关系不明确时，可以采用下面的两种写法，但是第一种写法B组件无法拿到A组件中的数据，使用第二种写法可以传值给子组件
<A>
    <B></B>
</A>
{{this.props.children}}    

<A render = {(data) => <B data={data}/>}>
// A组件中，在B组件中就可以{this.props.data}读取数据了
{this.props.render(内部state数据)}
```

8. 错误边界

用来捕获后代组件错误，渲染出备用页面。只能处理生命周期函数

```javascript
state = {
	hasError:'' //用于标识子组件是否产生错误
}
//当Parent的子组件出现报错时候，会触发getDerivedStateFromError调用，并携带错误信息
static getDerivedStateFromError(error){
	console.log('@@@',error);
	return {hasError:error}
}
```

9. 组件之间通信方式

> 父子组件：使用props
>
> 兄弟组件：消息订阅-发布、集中式管理redux
>
> 祖孙组件：消息订阅-发布、集中式管理redux、context


