

# React脚手架

```jsx
//安装脚手架
yarn add react-app-rewired --save-dev
npm install -g create-react-app

// 创建脚手架
create-react-app my-name(不能大写)
```



# 唯一key：

```js
const newID = Math.random().toString(36).substr(2);
```

# react生命周期

```jsx
  // 挂载时调用
  componentDidMount() {
    console.log('componentDidMount方法')
     // 此方法适用于发送网络请求（官方建议）
     // 也可以添加一些订阅（会在componentWillUnmount(销毁时)取消订阅）
  }

  // 更新时调用
  componentDidUpdate() {
    console.log('componentDidUpdate方法')	
      // 当组件更新后，可以在此处对DOM进行操作;
	  // 如果你对更新前后的props进行了比较，也可以选择在此处进行网络请求;(例如，当props未发生变化时，则不会执行网络请求)。
  }

  // 销毁时调用
  componentWillUNmount() {
    // 在此方法中执行必要的清理操作
    // 列如：定时器，componentDidMount()中创建的订阅等。
  }
```



# 父传子(class)

```jsx
import React from 'react'

// 父组件
export default class Dpp extends React.Component{
  constructor(){
    super()
    this.state = {
      isShow:'父组件的数据'
    }
  }

  render(){
    const {isShow} = this.state
    return(
      <div>
        <Ass isShow={isShow}/>
      </div>
    )
  }
}

// 子组件
class Ass extends React.Component{
  constructor(props){
    super(props)
    this.state={
      data:props.isShow
    }

  }

  render(){
    const {data} = this.state
    // const {isShow} = this.props
    return(
      <div>
        <p>数据：{ data }</p>
      </div>
    )
  }
}
```



# 父传子(function)

```jsx

// 父组件
export function Lpp(){
  
  const data = [
    {name:'(fun)父组件的数据',age:15},
    {name:'(fun)父组件null',age:18},
  ]
  return( 
    <div>
      <Kpp data={data}/>
    </div>
  )
}

// 子组件
function Kpp(props){
  console.log(props);
  return(
    <div>
      <p>(fun组件)数据：{props.data[1].name}</p>
    </div>
  )
}
```



# 子传父(class)

```jsx
// 父组件
import React, { Component } from 'react';
import App from './App'

export default class index extends Component {
  state = {
    titles: ['新歌', '精选', '流行', 'new'],
    aticon: 'aticon',
    data:'新款'
  }
  render() {
    const { titles, aticon ,data} = this.state;
    return (
      <div>
        <App titles={titles} ads={ i => this.ads(i)} aticon={aticon} />
        <h1>{data}</h1>
      </div>
    );
  }
  ads(index){	// 接受子组件传过来的索引参数
    console.log(index);
    const {titles} = this.state
    this.setState({
      data:titles[index]	// 根据索引参数来显示对应的数组
    })
  }
}



// 子组件
import React, { Component } from 'react';
import propTypes from 'prop-types'

export default class App extends Component {
  state={
    lei:0
  }
  render() {
    const {titles,aticon} = this.props
    const {lei} = this.state
    return (
      <div id="titles">
        {
          titles.map((item,index) => {
            return (
              <div key={index} 
                   id='titl' 
                   className={index === lei ? aticon:''} 
                   onClick={e => this.itemClick(index)}>
                     <span>{item}</span>
              </div>
            )
          })
        }
      </div>
    );
  }
  itemClick(index){
    this.setState({
      lei:index
    })
    this.props.ads(index) // 把index传去父组件
  }
}

// 类型限制
App.propTypes ={
  titles:propTypes.array.isRequired
}

```



# 跨组件传值(class & function)

```jsx
import React, { Component } from 'react'

const Initialvalue = {	// 默认值
  name: '--------!!!'	
}
const Consumers = React.createContext(Initialvalue)

// 父组件
export default class NewContext extends Component {
  state = {
    name: 'chen zhan hong'
  }
  render() {
    return (
      <div>
        <Consumers.Provider value={this.state}>
          <h1>父组件：{this.state.name}</h1>
          <NewContextZi />
        </Consumers.Provider>
      </div>
    )
  }
}

// 子组件
class NewContextZi extends Component {
  render() {
    return (
      <Consumers.Consumer>
        {
          vla => {
            return (
              <div>
                <h2>子组件：{vla.name || 0}</h2>
                <NewContextSUN />
              </div>
            )
          }
        }
      </Consumers.Consumer>
    )
  }
}

// 孙组件
class NewContextSUN extends Component {
  render() {
    console.log(this);
    return (
      <div>
        <h3>孙组件：{this.context.name}</h3>
        <NewContextZengSUN />
      </div>
    )
  }
}

NewContextSUN.contextType = Consumers

// 曾孙组件
function NewContextZengSUN(props) {
  return (
    <Consumers.Consumer>
      {
        vla => {
          console.log(vla);
          return (
            <h4>曾组件：{vla.name || 0}</h4>
          )
        }
      }
    </Consumers.Consumer>
  )
}

```



# setState(class )

```jsx
import React, { Component } from 'react'

export default class App extends Component {
  state = {
    data: 0
  }
  render() {
    return (
      <div>
        <h1>{this.state.data}</h1>
        <button onClick={() => this.dianjin()}>+1</button>
      </div>
    )
  }
  dianjin() {
      // setState(更新数据, 回调函数)
      // setState是异步的
    this.setState({
      data: this.state.data + 1
    }, e => {
      console.log(this.state.data);	// 拿到更新之后的值
    })
  `dianjin() {
      // propsState 更新之前的值
    this.setState((propsState,p) => ({data:propsState.data+1}))
  }`
  }
}

```



# ref(class)获取DOM元素

```jsx
import React, { PureComponent,createRef } from 'react'

class Cevent extends PureComponent {
  titleREF = createRef();
  titFunRef = null;

  render(){
    return(
      <div>
         {/* ref是拿到DOM元素的 */}
        <p ref = 'czh'>Cevent</p>
        <p ref = { this.titleREF } >Cevent</p>
        <p ref = { e => this.titFunRef = e }>Cevent</p>
        <button onClick={() => this.Evenetsemit()}>点击了Cevent</button>
      </div>
    )
  }
  Evenetsemit(){
      {/* 使用方式一：字符串方式（官方不推荐使用，后期会废气掉）*/}
    this.refs.czh.innerHTML = 'hello'
      {/* 使用方式二：对象方式（目前官方推荐使用）*/}
    this.titleREF.current.innerHTML = 'hello createRef'
      {/* 使用方式三：回调函数方式*/}  
    this.titFunRef.innerHTML = 'hello FunRef'
    console.log(this.refs.czh);
  }
}
```



# ref(function)获取DOM元素

```jsx
import React, { forwardRef, PureComponent, createRef} from 'react'


export default class Ref extends PureComponent {
  createRefs = createRef()
  render() {
    const cnm = () => {
      return (e) => {
        console.log(this.createRefs.current);
      }
    }
    return (
      <div>
        <Context ref={this.createRefs} />
        <button onClick={cnm(this)}>打印ref</button>
      </div>
    )
  }
}

export const Context = forwardRef(
  (props, ref) => {
    console.log(props);
    return (
      <div>
        <p id='ss' ref={ref}>Context</p>
      </div>
    )
  }
)

```



# CSS in JS

```jsx

// 第一种
<p style={{fontSize:"18px",color:"red"}}>内联样式</p>


// 第二种
const styles = {
        fontSize:"18px",
        color:this.state.style
    }
return(
	<p style={styles}>动态内联样式</p> 
)


// 第三种：创建一个App.module.css样式文件并引入
App.module.css
	.nameclass{
     	color:'red'  
    }
App.module.css

impot AppStyle from './App.module.css'

render(){
 return(
 	<div>
        <p className = {AppStyle.nameclass}>module.css样式</p>
    </div>
 )   
}

// 第四种 CSS in js
yarn add styled-components
vscode-styled-components vscode安装这个代码提示插件
import styled from 'styled-components'
const Styless = styled.div`
	color: red;
	font-size: 18px;
`
```



# axios基本使用

```jsx
componentDidMount(){
    // get
    axios({
      url:'http://www.httpbin.org/get',
      params:{
        name:'浪白',
        age:18
      }
    })
    .then(d => console.log(d))
    .catch(e => console.log(e))

    // post
    axios({
      url:'http://www.httpbin.org/post',
      data:{
        name:'post浪白',
        age:19
      },
      method:'post'

    })  
    .then( d => console.log(d))
    .catch( e => console.log(e))
    
    axios.get('http://www.httpbin.org/get',{
      params:{
          name:'czh',
          age:20
      }
    }).then(e => console.log(e))
    .catch(e => console.log(e))
    
    axios.post('http://www.httpbin.org/post',
      {
        name:'czh-post',
        age:20
      }
    ).then(e => console.log(e))
    .catch(e => console.log(e))
  }


// 此代码代替上面代码（建议使用这个）
// async (异步)	await (等待)
// try是检测错误 catch是拿到检测的错误信息
// 使用await的前提得是在async里面 列如下面代码
 async componentDidMount(){
   try{
 	const result = await axios.post('http://www.httpbin.org/post',
      {
        name:'czh-post',
        age:20
      }
    )
 	console.log(result)//拿到数据
    }catch(err){
      console.log(err);//拿到错误
    }
}
```



# 过度动画和纯函数使用

```jsx
yarn add react-transition-group

react-transition-group主要包含四个组件：
Transition
	口该组件是一个和平台无关的组件(不一定要结合CSS ) ;
	在前端开发中，我们一般是结合CSS来完成样式，所以比较常用的是
CSSTransition 
	在前端开发中，通常使用CSSTransition来完成过渡动画效果
SwitchTransition
	两个组件显示和隐藏切换时，使用该组件TransitionGroup
	将多个动画组件包裹在其中，一般用于列表中元素的动画;
TransitionGroup
	

```



## 1.CSSTransition：

```css
CsSTransition执行过程中，有三个状态: appear、enter、exit ;
它们有三种状态，需要定义对应的CSS样式:
第一类：开始状态:对于的类是: 	card-appear   		card-enter   		card-exit ;
第二类：执行动画:对应的类是:  card-appear-active   card-enter-active   card-exit-active 
第三类：执行结束:对应的类是:  card-appear-done     card-enter-done     card-exit-done ;

//开始时(页面挂载时)
.card-appear, .card-enter{
  opacity: 0;
  transform: scale(0.9);
}
//执行时
.card-appear-active, .card-enter-active{
  opacity: 2;
  transform: scale(1);
  transition: opacity 300ms, transform 300ms;
}
//结束时
.card-appear-done, .card-enter-active{
  
}
...
CSSTarnsition的钩子函数：
 	onEnter = { e => console.log('开始挂载')}
 	onEntering = { e => console.log('挂载中')}
 	onEntered = { e => console.log('挂载完毕')}
 	onExit = { e => console.log('开始退出')}
 	onExiting = { e => console.log('退出中')}
 	onExited = { e => console.log('退出完毕')}
// jsx
<CSSTransition
	in={isShow} 
    classNames='card'	/
    timeout={300}
    unmountOnExit={true}
    appear
    onEnter = { e => console.log('开始挂载')}
    onEntering = { e => console.log('挂载中')}
    onEntered = { e => console.log('挂载完毕')}
    onExit = { e => console.log('开始退出')}
    onExiting = { e => console.log('退出中')}
    onExited = { e => console.log('退出完毕')}
>
....

</CSSTransition>

```

## 2.SwitchTransition：

```css

```



# Hooks-redux

```jsx
yarn add redux


// incs.js 公共代码
export const INCREMENT = 'INCREMENT';
export const DCEREMENT = 'DCEREMENT';


// store/reducer.js  业务逻辑代码
import { INCREMENT, DCEREMENT } from './incs.js'
const initialState = {
  counter: 0
}
export default function reducer(state = initialState,action){
  switch (action.type) {
    case INCREMENT:
      return {...state,counter:state.counter + action.nub};
    default:
      return  state;
  }
}


// store/index.js
import { createStore }  from 'redux';
import reduxer from './reducer.js'
export  const store = createStore(reduxer);


// action.js
import { INCREMENT } from './store/incs.js'
export const addAction = nub => {
  return {
    type: INCREMENT,
    nub
  }
}


//	index.js
import { store } from './store/index.js'
import { addAction } from './actionVreatore.js'
// 订阅（监听store值得改变）
store.subscribe(() => {
  console.log(store.getState())
})
// 派发
store.dispatch(addAction(10))
```



# redux基本使用

```jsx
// redux/incs.js(公共文件)
const INCADDCON = 'INCADDCON'
const DELTSADDS = 'DELTSADDS'
export {
  INCADDCON,
  DELTSADDS
}


// redux/reducer.js
import { INCADDCON, DELTSADDS } from './incs'
const NEWobj = {
  counter: 0
}
export default function reducer(state = NEWobj, action) {
  const { type, Nans } = action;
  switch (type) {
    case INCADDCON:
      return { ...state, counter: state.counter + Nans }
    case DELTSADDS:
      return { ...state, counter: state.counter - Nans }
    default:
      return state;
  }
}

// redux/store.js
import { createStore } from 'redux'
import reducer from './reducer'
export const store = createStore(reducer)


// redux/action.js
import { INCADDCON,DELTSADDS } from './incs'
const Addaction = Nans => {
  return {
    type: INCADDCON,
    Nans
  }
}
const DELaction = Nans => {
  return {
    type: DELTSADDS,
    Nans
  }
}
export {
  Addaction,
  DELaction
}

// app.js
import React, { PureComponent } from 'react'
import { store } from '../store/store'
import { Addaction, DELaction} from '../store/action'

export default class index extends PureComponent {
  state ={
    cao: store.getState().counter
  }

  // dispatch: ƒ dispatch(action)
  // getState: ƒ getState()
  // subscribe: ƒ subscribe(listener)
  componentDidMount(){
    store.subscribe(e=>{// 挂载时订阅store的改变
      this.Cancelsubscribe = this.setState({
        cao:store.getState().counter
      })
    })
  }
  componentWillUnmount(){
    this.Cancelsubscribe()// 卸载时取消订阅
  }

  add = e => store.dispatch(Addaction(1)) 
  Del = e => store.dispatch(DELaction(1)) 

  render() {
    console.log(store)
    const { cao } = this.state
    return (
      <div>
        <h1>当前计数：{cao}</h1>
        <button onClick= {this.add}>+1</button>
        <button onClick= {this.Del}>-1</button>
      </div>
    )
  }
}

```



# redux中间件使用

```jsx
// 下载中间件
yarn add redux-devtools-extension redux-thunk

// 第一种方法
import { createStore, applyMiddleware } from 'redux'
import thunk from 'redux-thunk'	// 重点
import { composeWithDevTools } from 'redux-devtools-extension'	// 重点
import {redux} from './reducer'
export default createStore(redux, composeWithDevTools(applyMiddleware(thunk)))

// 第二种方法
import { createStore,applyMiddleware, compose } from 'redux'
import thunk from 'redux-thunk' 
import {redux} from './reducer'
const composeEnhancers = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ || compose;
export default createStore(redux, composeEnhancers(applyMiddleware(thunk)))
```



# react-redux的使用

```jsx
yarn add redux
yarn add react-redux

1.定义action.type
const IN = 'IN'
const DN = 'DN'
export { IN, DN }

2.定义action
import {IN,DN} from './INDN'
const Add = ()=> ({ type:IN })  
const Ddd = ()=>{
  return {
    type:DN
  }
}
export {  Add, Ddd }


3.定义reducer
import { IN, DN } from './INDN'
const data = { value:'初始值' }

export default function reducer(state = data, action) {
  console.info('reducer:',action,state)
  switch (action.type) {
    case IN:
      return {name:'你好'} 
    default:
      return state;
  } 
}


4.定义store
import { createStore } from 'redux'
import reducer from './reducer'
export const store = createStore(reducer)


5.index.jsx
// 使用Provider向每个子组件传递store
import React from 'react'
import { Provider } from 'react-redux'
import {store} from './store'
import App from './App'
import Bpp from './Bpp'
export default function index() {
  return (
    <Provider store = { store } >
      <App/>
      <Bpp/>
    </Provider>
  )
}


6.子组件App
import React from 'react'
import { connect } from 'react-redux'
import {Add,Ddd} from './action'
function App(porps) {
  const ASD = () => porps.sendAdd()
  console.info(porps)
  return (
    <div>
      <button onClick = {ASD}>....</button>
    </div>
  )
}
const mapDispatchToProps = (dispatch) =>{
  return {
    sendAdd: () => { // sendAdd方法会传到props
      dispatch(Add())
    }
  }
}
export default connect(null,mapDispatchToProps)(App)	// App是发送方所以connect第一个参数是null


7.子组件Bpp
import React from 'react'
import {connect} from 'react-redux'
function Bpp(props) {
  console.info('Bpp',props)
  return (
    <div>
      <p>value:{props.data}</p>
    </div>
  )
}
  const mapStateToProps = state => {
    return {data:state.name}	// data会传到props
  }
export default connect(mapStateToProps,null)(Bpp)  // Bpp是接收方所以connect第二个参数是null
```

# redux-combineReducers

```jsx
yarn add redux
yarn add react-redux

1.定义action.type
const IN = 'IN'
const DN = 'DN'
export { IN, DN }

2.定义action
import {IN,DN} from './INDN'
const Add = ()=> ({ type:IN })  
const Ddd = ()=>{
  return {
    type:DN
  }
}
export {  Add, Ddd }


3.定义reducer
import { IN, DN } from './INDN'
const data = { value:'初始值' }

export default function reducer(state = data, action) {
  console.info('reducer:',action,state)
  switch (action.type) {
    case IN:
      return {name:'你好'} 
    default:
      return state;
  } 
}


4.定义store
import { createStore, combineReducers } from 'redux'
import reducer from './reducer'
const redux = combineReducers({	// combineReducers作用是把所有子的reducer合并
  user:reducer
})
export const store = createStore(redux)

5.index.jsx
// 使用Provider向每个子组件传递store
import React from 'react'
import { Provider } from 'react-redux'
import {store} from './store'
import App from './App'
import Bpp from './Bpp'
export default function index() {
  return (
    <Provider store = { store } >
      <App/>
      <Bpp/>
    </Provider>
  )
}


6.子组件App
import React from 'react'
import { connect } from 'react-redux'
import {Add,Ddd} from './action'
function App(porps) {
  const ASD = () => porps.sendAdd()
  console.info(porps)
  return (
    <div>
      <button onClick = {ASD}>....</button>
    </div>
  )
}
const mapDispatchToProps = (dispatch) =>{
  return {
    sendAdd: () => { // sendAdd方法会传到props
      dispatch(Add())
    }
  }
}
export default connect(null,mapDispatchToProps)(App)	// App是发送方所以connect第一个参数是null


7.子组件Bpp
import React from 'react'
import {connect} from 'react-redux'
function Bpp(props) {
  console.info('Bpp',props)
  return (
    <div>
      <p>value:{props.data}</p>
    </div>
  )
}
  const mapStateToProps = state => {
    return {data:state.name}	// data会传到props
  }
export default connect(mapStateToProps,null)(Bpp)  // Bpp是接收方所以connect第二个参数是null

```



# react-redux(Hooks)

```jsx
yarn add redux
yarn add react-redux

1.定义action.type
const IN = 'IN'
const DN = 'DN'
export { IN, DN }

2.定义action
import {IN,DN} from './INDN'
const Add = ()=> ({ type:IN })  
const Ddd = ()=>{
  return {
    type:DN
  }
}
export {  Add, Ddd }


3.定义reducer
import { IN, DN } from './INDN'
const data = { value:'初始值' }

export default function reducer(state = data, action) {
  console.info('reducer:',action,state)
  switch (action.type) {
    case IN:
      return {name:'你好'} 
    default:
      return state;
  } 
}


4.定义store
import { createStore, combineReducers } from 'redux'
import reducer from './reducer'
const redux = combineReducers({	// combineReducers作用是把所有子的reducer合并
  user:reducer
})
export const store = createStore(redux)

5.index.jsx
// 使用Provider向每个子组件传递store
import React from 'react'
import { Provider } from 'react-redux'
import {store} from './store'
import App from './App'
import Bpp from './Bpp'
export default function index() {
  return (
    <Provider store = { store } >
      <App/>
      <Bpp/>
    </Provider>
  )
}


6.子组件App
import React,{ useEffect } from 'react'
import { useDispatch } from 'react-redux'  
import {Add,Ddd} from './action'

export default function App() {
  
  const dispatch = useDispatch();	// 代替 mapDispatchToProps()
  useEffect( () => {
    dispatch(Add())
  }, [dispatch])
  return (
    <div>
      <button onClick = {ASD}>....</button>
    </div>
  )
} // App是发送方


7.子组件Bpp
import React from 'react'
import { useSelector, shallowEqual } from 'react-redux'
function Bpp(props) { 
  
  const { data } = useSelector(() => ({	// 代替 mapStateToProps()
   data: user.name
  }), shallowEqual)
  
  return (
    <div>
      <p>value:{data}</p>
    </div>
  )
}  // Bpp是接收方

 
```



# redux-persist

```jsx
// redux-persist持久化状态
npm install redux-persist --save
cnpm install --save redux-persist 
yarn add redux-persist -S




```





# immutable.js

```js
<script src="https://cdnjs.cloudflare.com/ajax/libs/immutable/4.0.0/immutable.min.js"></script>
  <script>
  // set('key','value') 修改
  // get('key') 获取
    // Map的使用
    const im = Immutable;
    const ion = {
      name:'kobe',
      age:'15',
      fun:{
        name: 'json',
        age:44
      }

    }
    // const newIm = im.Map(ion)//浅层转换
    const newIm = im.fromJS(ion)//深层装换

    const newIon = newIm 
    const newIon2 = newIm.set('name','set科比') 
    console.info('newIon:',newIon.get('name'))
    console.info('newIon2:',newIon2.get("name"))   
    console.info('im:',newIon2.get('fun'))

    // List
    const arrays = ['abc','cba','bba']
    const nameArr = im.List(arrays)
    const arr2 = nameArr.set(0,'List')
    console.info(nameArr.get(0))
    console.info(arr2.get(0)) 
  </script>
```



# React-Router

```jsx
yarn add react-router-dom
```

```jsx
react-router最主要的API是给我们提供的一些组件
BrowserRouter或HashRouter：
    Router中包含了对路径改变的监听，并且会将相应的路径传递给子组件;
    BrowserRouter使用history模式;
    HashRouter使用hash模式;

Link和NavLink
    通常路径的跳转是使用Link组件，最终会被渲染成a元素;
    NavLink是在Link基础之上增加了一些样式属性（后续学习);
    to属性:Link中最重要的属性，用于设置跳转到的路径;

Route:
    Route:用于路径的匹配;
    path属性∶用于设置匹配到的路径;
    component属性∶设置匹配到路径后，渲染的组件;
    exact:精准匹配，只有精准匹配到完全一致的路径，才会渲染对应的组件;




```



## 1.Router基本使用：

```jsx
import React, { PureComponent ,createRef} from 'react'

import {
  HashRouter,
  Link,
  Route,
  Switch
} from 'react-router-dom'

export default class index extends PureComponent {
  state={
    ref: createRef()
  }
  render() {
    return (
      <>
        <Link to='/'>首页</Link>
        <Link to='/abuot'>关于</Link>
        <Link to='/profile'>我的</Link>
        <Switch>
         	<Route exact path="/" component={Sho}/>
          <Route path="/abuot" component={Abuot}/>
          <Route path="/profile" component={Bass}/>
          <Route component={Lass}/>
        </Switch>
      </>
    )
  }
  
}
class Lass extends PureComponent{
  render(){
    return (
      <div>
        首页
      </div>
    )
  }
}

class Sho extends PureComponent{
  render(){
    return (
      <div>
        首页
      </div>
    )
  }
}

class Bass extends PureComponent {
  render() {
    return (
      <div>
        Bass
      </div>
    )
  }
}
class Abuot extends PureComponent {
  render() {
    return (
      <div>
        abuot
      </div>
    )
  }
}

```

## 2.手动Router：

```jsx
本节重点 withRouter：
withRouter 都会将 updated match、location 和 history props 传递给它包装的组件

import React, { PureComponent } from 'react'

import {
  BrowserRouter,
  Route,
  Switch,
  withRouter
} from 'react-router-dom'

class index extends PureComponent {
  render() {
    return (
      <>
        <Link to='/'>首页</Link>
        <button onClick = {this.Sh}>商品列表</button>
        <Switch>
         	<Route exact path="/" component={Sho}/>
          <Route path="/Sh" component={Sh}/>
          <Route component={Lass}/>{/*当路由全部不匹配的时候就显示该*/}
        </Switch>
      </>
    )
  }
  Sh = e => {
    this.props.history.push('/Sh')
    console.log(this.props)
  }
}
export default withRouter(index);


class Sho extends PureComponent{
  render(){
    return (
      <div>
        首页
      </div>
    )
  }
}

class Sh extends Component {
  render() {
    return (
      <div>
        <ul>
          <li>商品1</li>
          <li>商品2</li>
          <li>商品3</li>
        </ul>
      </div>
    )
  }
}


```

## 3.动态Router：

```jsx

import React, { PureComponent } from 'react'

import {
  BrowserRouter,
  Route,
  Switch,
  withRouter
} from 'react-router-dom'

class index extends PureComponent {
  render() {
      const id = 'zbc'
    return (
      <>
        <Link to='/'>首页</Link>
        <NavLink to={`/xiangxin/${id}`} >详细</NavLink>
        <Switch>
         	<Route exact path="/" component={Sho}/>
          <Route path="/xiangxin/:id" component={Xiang}/>
        </Switch>
      </>
    )
  }
}
export default withRouter(index);


class Sho extends PureComponent{
  render(){
    return (
      <div>
        首页
      </div>
    )
  }
}


class Xiang extends PureComponent{
  render(){
    const id = this.props.match.params.id
    console.log('拿到动态id',id)
    return(
      <div>
        <ul>
          <li>详细{id}</li>
          <li>详细2</li>
          <li>详细3</li>
        </ul>
      </div>
    )
  }
}
```

## 4.集中Router管理：

```jsx
yarn add react-router-config

重点：
	集中路由配置完成后就在组件中使用 router-config 的renderRoutes 来包裹集中路由 renderRouts(routes)
    子路由这样被包裹 renderRoutes(this.props.route.routes)

新建一个 routers 文件夹再建一个index文件
// index.jsx：
import Abuot from './component/abuot'
import Toab from './component/toab'
import Index from './component/index'

export const routes = [
 {
   exact: true,
   path: '/',
   component: Index
 },
 {
   path: '/abuot',
   component: Abuot,
   routes:[	// 子路由
      {
        path: '/abuot/toab',
        component: Toab
      }
   ]  
 } 
]
// index.jsx:
import { renderrRoutes } from 'react-router-config'
import { routes } from './routers'
return(
	<div>
        <NavLink to='/abuot'>关于</NavLink>
        { renderRoutes(routes) }
    </div>
)

// Abuot.jsx:
return(
	<div>
        <NavLink to='/abuot/toab'>二级路由</NavLink>
        { renderRoutes(this.props.route.routes) }
    </div>
)

// Toab.jsx:
return(
	<div>
        <p>Toab</p>
    </div>
)

// v6
// routers.js
import { useRoutes, Navigate } from 'react-router-dom'
// 使用useRoutes前必须被一个函数包裹着
export const headRoutes = _=> {
  const route = useRoutes([
    {
      path: '/',
      element: <Navigate  to = 'mine' />//重定向
    },
    {
      path: '/mine',
      element: <Navigate to = 'recommend' />//重定向
    },
    {
      path: '/mine',
      element: <Mine />,
      children: [
        { path: 'recommend', element: <Recommend/> },
      ]
    }
  ])
  return route
}
 
//App.js
import {HeadRoutes} from '/routers.js'
function App(){
  return (
  	<div>
    	<HeadRoutes/>
    </div>
  )
}

```



# Hooks-useState

```jsx
// useState( 0,fn ) 返回两个数组：
//   第一个参数是默认值，也可以是函数和数组 useState( () => 10 )，
//   第二个参数是改变第一个参数的值的函数

import React, { useState } from 'react'

export default function Hooks(){
	
//const [状态, 改变状态的方法] = useState(状态)
  const [count, setCount] = useState(0)
  
  return (
    <div>
      <h1>当前求和：{count}</h1>
      <button onClick={ () => setCount( count + 1 ) }>+1</button>
    </div>
  )
}


export default function useState_fun() {
  const [ count, setCou ] = useState(() => 10) 
  const setCoufun = () => {
    setCou(Previouscou => Previouscou + 10)
  }
  return (
    <div>
      <h1>函数useState:</h1>
      <h4>{count}</h4>
      <button onClick = { setCoufun } >++</button>
    </div>
  )
}
```

## 1.复杂版useState处理

```jsx
import React, { useState } from 'react'

export default function Hokks() {
  const [state, setState] = useState(0);
  const initialState = [
    { id: 101, name: '李雷', age: 15},
    { id: 102, name: '赵信', age: 50},
    { id: 103, name: '张三', age: 30},
  ]
  const [arrAy, setArrAy] = useState(initialState)

  function add(i){
    const arr = [...arrAy]
    arr[i].age += 1
    setArrAy(arr)
  }
  return (
    <div>
      <h1>当前求和：{state}</h1>
      <button onClick={() => setState(state + 1)}>+1</button>
      <hr />
      <h1>好友列表</h1>
      <ul>
        {
          arrAy.map((item,index) => {
            return (
              <li key = {item.id}>
                <span>姓名：{item.name} 年龄：{item.age}</span>
                <button onClick = {e => add(index)}>年龄+1</button>
              </li>
            )
          })
        }
      </ul>
    </div>
  )
}
```





# Hooks-useEffect

```jsx
{/* 
	useEFFect(fn,[])
    参数一：必传
    参数二：可选 (不传的话，组件一更新就调用 useEffect)
    	如果是空数组的话表示谁都不依赖只在第一次渲染时调用一次
    	如果数组里有依赖一些变量的话，变量发生变化就会调用
*/}
useEffect(() => {
  effect
  return () => {
    cleanup
  }
}, [])

```

## 1.useEffect基本使用

```jsx
import React,{ useState, useEffect } from 'react'

export default function useState_fun() {
  const [ count, setCou ] = useState(() => 10) 
  const setCoufun = () => {
    setCou(Previouscou => Previouscou + 10)
  }

  useEffect( () => {
    document.title = count
  })
  return (
    <div>
      <h1>函数useState:</h1>
      <h4>{count}</h4>
      <button onClick = { setCoufun } >++</button>
    </div>
  )
}
```

## 1.useEffect的订阅和取消订阅

```jsx
import React,{ useState, useEffect } from 'react'

export default function useState_fun() {
    
  const show = true
  const [ counts, setCous ] = useState(show) 
  const setCoufuns = () => {
    setCous( coun => !coun )
  }
  console.log(counts);
  return (
    <div>
      <h1>订阅和取消订阅:</h1>
      <button onClick = { setCoufuns } >切换</button>
      {
        // 逻辑与：只判断前面是否有true
       	// 等同 counts ? <Add/> : null
        counts && <Add/>
      }
    </div>
  )
}

function Add(){
  useEffect(()=>{
    console.log('订阅事件')
    return () => {
      console.log('取消订阅')
    }
  },[])
  return(
    <div>Effect的订阅和取消订阅</div>
  )
}
```

## 1.多个useEffect

```jsx
import React,{ useState, useEffect } from 'react'

export default function Add(){
  useEffect(()=>{
    console.log('修改dom')
    return () => {
      console.log('取消订阅')
    }
  },[])
    useEffect(()=>{
    console.log('订阅事件')
    return () => {
      console.log('取消订阅')
    }
  },[])
    useEffect(()=>{
    console.log('网络请求')
    return () => {
      console.log('取消订阅')
    }
  },[])
  return(
    <div>Effect的订阅和取消订阅</div>
  )
}
```



# Hooks-useContext

```jsx
const themes = {
  light: {
    foreground: "#000000",
    background: "#eeeeee"
  },
  dark: {
    foreground: "#ffffff",
    background: "#222222"
  }
};

export const ThemeContext = React.createContext(themes.light);

function App() {
  return (
    <ThemeContext.Provider value={themes.dark}>
      <Toolbar />
    </ThemeContext.Provider>
  );
}

function Toolbar(props) {
  return (
    <div>
      <ThemedButton />
    </div>
  );
}

function ThemedButton() {
  const theme = useContext(ThemeContext);
  return (
    <button style={{ background: theme.background, color: theme.foreground }}>
      I am styled by theme context!
    </button>
  );
}
```

## 1.

```jsx

```



# Hooks-useReducer

```jsx
{/*
	和redux很像但又不像，
	所以，useReducer只是useState的一种替代品，并不能替代Redux。
*/}

function reducer(state, action){
  switch(action.type){
    case 'incermenet':
      return { ...state, count:state.count + 1}
    case 'dncermenet':
      return { ...state, count:state.count - 1} 
    default:
      return state
  }
}

function UseReducss(){
  const [ useStat, dispatch ] = React.useReducer(reducer,{count:0})
  return (
    <div>
      <h1>当前计数：{ useStat.count }</h1>
      <button onClick={ () => dispatch({type:'incermenet'})}>+1</button>
      <button onClick={ () => dispatch({type:'dncermenet'})}>-1</button>
    </div>
  )
}
```

## 1.

```jsx

```



# Hooks-useRef

```jsx
import React,{ useRef } from 'react'

export default function HooksuseRef() {
  const titleRef = useRef();
  const counRef = useRef();

  const chengDOM = e => {
    titleRef.current.innerHTML = 'Hello useRef'
    counRef.current.focus()
  }
  return (
    <div>
      <h1 ref = {titleRef}>useRef</h1>
      <input ref = {counRef} type="text" />
      <button onClick = {chengDOM}>修改DOM</button>
    </div>
  )
}

```

## 1.

```jsx
import React,{ useRef } from 'react'

export default function HooksuseRef() {
  const [count, setCount] = React.useState(0)
  const RefCount =  useRef(count)
  
  const chengDOM = e => {
    setCount(count + 10)
  }
  
  React.useEffect(()=>{
    RefCount.current = count
  },[count])
  return (
    <div>
      <h1>前一次的的值：{RefCount.current}</h1>
      <h1>最新的值：{count}</h1>
      <button onClick = {chengDOM}>修改DOM</button>
    </div>
  )
}

```



# forwardRef

```jsx

import React from 'react'
const NforWard = React.forwardRef( (props,ref) => {
  console.log(ref);
  return (
    <input ref = {ref} type="text" />
  )
})

export default function ForwardRef() {
  const forwardref = React.useRef()
  return (
    <div>
      <NforWard ref = { forwardref } />
      <button onClick = { () => forwardref.current.focus() }  >聚焦</button>
    </div>
  )
}

```

## 1.

```jsx

```



# Hooks-useImperativeHandle

```jsx

{/*
	官方解释：
	useImperativeHandle 可以让你在使用 ref 时自定义暴露给父组件的实例值。在大多数情况下，应当避免使用 	  ref 这样的命令式代码。useImperativeHandle 应当与 forwardRef 一起使用：
*/}

import React,{useRef, useImperativeHandle } from 'react'

const NforWards = React.forwardRef( (props,ref) => {
  const inputRef = useRef()
  
  useImperativeHandle(ref,()=>({
    focus:()=>{
      inputRef.current.focus()
      console.log(re.current,'Imperative');
    }
  }),[inputRef.current])
  console.log(ref);
  return (
    <input ref = {inputRef} type="text" />
  )
})

function ForwardRefs() {
  const forwardref = React.useRef()
  console.log(forwardref);
  return (
    <div>
      <NforWards ref = { forwardref } />
      <button onClick = { () => forwardref.current.focus() }  >聚焦</button>
    </div>
  )
}
```

## 1.

```jsx

```



# Hooks-useLayoutEffect

```jsx

import React,{ useLayoutEffect } from 'react'
function ForwardRefs() {
  const [ counts, setCount ] = React.useState(10)
  
  useLayoutEffect(() => {
    if(counts === 0){
      setCount(Math.random())
    }
  }, [counts])
    
  return (
    <div>
      <h1>{counts}</h1>
      <button onClick = {() => setCount(0)}>++</button>
    </div>
  )
}

```

## 1.

```jsx

```



# 自定义Hooks

```jsx
import React,{ useEffect,useState } from 'react'

export default function ZHooks() {
  const [ state, setStates ] = useState(true)
  useHOOKS('ZHooks')

  return (
    <div>
      { state && <Bpp/> }
      <button onClick = { e => setStates(!state) } >切换</button>
    </div>
  )
}

function Bpp(){
  useHOOKS('Bpp')
  return (
    <div>
      Bpp
      <Dpp/>
    </div>
  )
}

function Dpp(){
 useHOOKS('Dpp')
  return (
    <div>
      Dpp
    </div>
  )
}

function useHOOKS(name){// 自定义hooks必须前面加个use
  useEffect(()=>{
    console.log(`${name}挂载`);
    return ()=>{
      console.log(`${name}Dpp被销毁`);
    }
  },[])
}
```

## 1.自定义Hooks案例

```jsx
import React,{ createContext, useContext, useState } from 'react'

const count = createContext('默认值....')
const counts = createContext('默认值2....')

export default function ZHooks() {
  

  const [ state, setStates ] = useState(true)
  useHOOKS('ZHooks')

  return (
    <count.Provider value = {{name:'蜡笔小新',age:18}}>
      <counts.Provider value = {'lagnbai'}>
      { state && <Bpp/> }
      <button onClick = { e => setStates(!state) } >切换</button>
      </counts.Provider>
    </count.Provider>
  )
}

function Bpp(){
  useHOOKS('Bpp')
  return (
    <div>
      Bpp
      <Dpp/>
    </div>
  )
}

function Dpp(props){
 useHOOKS('Dpp')
 const [getCou, labi] = useUserContext()
 console.log(getCou,labi);
  return (
    <div>
      Dpp
    </div>
  )
}

function useUserContext(){	// 重点代码
  const getCou = useContext(count)
  const labi = useContext(counts)
  return [ getCou, labi ]
}

```

## 1.自定义Hooks案例2

```jsx
// 获取滚动位置
import React,{ useState, useEffect, } from 'react'

function Bpp() {
  const [state] = useUserContext()
    console.log(state);
  return (
    <div style={{ height: 1000 + "px" }}>
      <h2 style={{ position: 'fixed', left: 0, top: 0 }}>自定义Hooks练习：{state}</h2>
    </div>
  )
}
function useUserContext() {
  const [state,getState] = useState(0)
  useEffect(() => {
    document.addEventListener('scroll',()=>{
      getState(window.scrollY)
    })
  },[state])
  return [state]
}

```

# React SSR

```
使用React SSR主要有两种方式:
方式一︰手动搭建一个SSR框架;
方式二︰使用已经成熟的SSR框架:

Next.js安装Next.js框架的脚手架∶
npm install -g create-next-app

创建Next.js项目
create-next-app next-demo-name

查看版本号：
create-next-app --version


让 styled-components 在服务端生效必须做此操作
yarn add -D babel-plugin-styled-components
.babelrc
	{
		“presets”: [
			"next/babel"
		],
		"plugins": [
			["styled-components"]
		]
	}
.babelrc
```



# rem与px的转换

```jsx
 
rem是相对于根元素<html>，这样就意味着，我们只需要在根元素确定一个参考值，这个参考值设置为多少，完全可以根据您自己的需求来定。 我们知道，浏览器默认的字号16px，来看一些px单位与rem之间的转换关系：

|  px  |     rem       |
------------------------
|  12  | 12/16 = .75   |
|  14  | 14/16 = .875  |
|  16  | 16/16 = 1     |
|  18  | 18/16 = 1.125 |
|  20  | 20/16 = 1.25  |
|  24  | 24/16 = 1.5   |
|  30  | 30/16 = 1.875 |
|  36  | 36/16 = 2.25  |
|  42  | 42/16 = 2.625 |
|  48  | 48/16 = 3     |
-------------------------  
```



#  项目别名craco

```css
yarn add @craco/craco
  
/*craco.config.js*/
	
  const path = require('path');

  const resolve = dir => path.resolve(__dirname,dir);

  module.exports = {
    webpack: {
      alias: {
        "@": resolve("src"),
        "components": resolve("src/components")
      }
    }
  }
/*craco.config.js*/
  
```



