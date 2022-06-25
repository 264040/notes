# 1.useState

```jsx
// useState( 0,fn )
// 参数一是默认值，
// 参数二是改变第一个参数值的函数
import React, { useState } from 'react'

export default function Hooks(){
	
//const [状态, 改变状态的方法] = useState(状态)
// useState的返回值是两个数组：
//   参数一：当前的的状态（默认值）,
//	 参数二：改变状态的函数
  const [count, setCount] = useState(0)
  
  return (
    <div>
      <h1>当前求和：{count}</h1>
      <button onClick={ () => setCount( count + 1 ) }>+1</button>
    </div>
  )
}






```





# 2.useEffect

```jsx
useEffect 相当于 componentDidMount ==>挂载时  and  componentDidUpdate ==>更新完毕	


import React, { useState ,useEffect} from 'react'

export default function Hooks(){
	
  //const [状态, 改变状态的方法] = useState(状态)
  const [count, setCount] = useState(0)
  
  // 只要 count 发生变化 useEffect就再次执行 ,如果是 [ ] 那就执行一次。
  React.useEffect(()=>{
    console.log(`You clicked ${count} times`);
  },[count])
    
  return (
    <div>
      <h1>当前求和：{count}</h1>
      <button onClick={ () => setCount( count + 1 ) }>+1</button>
    </div>
  )
}




2.清除副作用
// 解绑时返回一个函数  相当于 componentWillUnmount==> 卸载
//	当count发生变化时会调用useEffect函数，函数内有返回值，返回值是清除副作用操作，里面可以写很多业务逻辑
React.useEffect(()=>{
    console.log(`You clicked ${count} times`);
    return () => {
      console.log('清除');
    }
  },[count])




提示:
如果你熟悉 React class 的生命周期函数，你可以把 useEffect Hook 
看做 componentDidMount，componentDidUpdate 和 componentWillUnmount 这三个函数的组合。

```



# 3.useContext

```jsx
向子组件传值

//父组件
import React from 'react'

import Bpp from './Bpp'

export const Countext = React.createContext()	// 创建并暴露上下文

 const useCstate = (initialValue)=> {
  const [count, setCount] = React.useState(initialValue)
  return [
    count,
    () => setCount(count + 1)
  ]
}
 
export default function Hooks(){

  const [count, setCount] = useCstate(0)
  
  return (
    <div>
      <h1>当前求和：{count}</h1>
      <button onClick={()=>{ setCount() }}>+1</button>
      <Countext.Provider value= {count} >	{/* 表明向 Bpp 组件提供 count 数据 */}
        {/* 把count传给子组件 */}
        {/* 如果要使用创建的上下文，需要通过 Countext.Provider 包装子组件，并且传入 value，指定要传入的值 */}
        <Bpp/>
      </Countext.Provider>
    </div>
  )
}

//子组件接收
import React ,{ useContext } from 'react'
import {Countext} from './Hooks'	//拿到父组件暴露出来的 Countext
export default function Bpp() {

//通过 React.createContext 创建出来的上下文，在子组件中可以通过 useContext 获取 Provider 提供的内容
  let conunt = useContext(Countext)
  return (
    <div>
      <h1>{ conunt }</h1>
    </div>
  )
}

```





# 4.useReducer

```jsx

/*incs组件*/
export const INCREMENT = 'INCREMENT'
export const DCEREMENT = 'DCEREMENT'
/*incs组件*/


import React, { useReducer ,createContext } from 'react'
import { INCREMENT, DCEREMENT } from './incs'

function usereducer(state, action) {
  switch (action.type) {
    case INCREMENT:
      return state + 1
    case DCEREMENT:
      return state - 1
    default:
      return state
  }
}

const initialState = 0

export default function Usereducer() {

  const [count, dispatch] = useReducer(usereducer, initialState)

  return (
    <div>
      <h1>Count: {count}</h1>
      <button onClick={() => dispatch({ type: DCEREMENT })}>-</button>
      <button onClick={() => dispatch({ type: INCREMENT })}>+</button>
    </div>
  )
}

```

