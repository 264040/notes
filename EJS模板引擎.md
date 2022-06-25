# EJS模板：

```js
yarn add ejs
 <!-- 
    ejs常用的三种写法：
      第一种：<% xxx %> xxx可以替换为任何js代码，不会输出任何内容给浏览器
      第二种：<%- xxx %> xxx可以替换为任何js变量，渲染标签
      第三种：<%= xxx %> xxx可以替换为任何js变量，不渲染标签
  -->
   

const { readFile } = require('fs')

const express = require('express')

const app = express()
// 设置模板引擎
app.set('view engine','ejs')
// 设置模板所在目录
app.set('views',__dirname + '/views')

app.get('/', (request,response) => {
  readFile('./data.json',( err,data ) => {
      // data = JSON.parse(data)
    // console.log(data)
    response.render('index',{data})
  })
  // const data = '<span>langbai</span>'
})

const port = 3656;
app.listen(port, err => {
  if(!err) console.log(`服务器http://localhost:${port}启动成功`)
  else console.log(err)
})

```



# cookie：

```js
1.是什么?
    本质就是一个字符串，里面包含着浏览器和服务器沟通的信息（交互时产生的信息〉。存储的形式以: key-value的形式存储。
    浏览器会自动携带该网站的cookie，只要是该网站下的cookie，全部携带。
    
2.分类：
	--会话cookie（关闭浏览器cookie也随之销毁，会话cookie储存在浏览器运行的那块内存）
    --持久化cookie（看过期时间，时间一到自动销毁，持久化cookie储存在用户的硬盘上）

const express = require('express')

const cookieParser = require('cookie-parser')

const app = express()

app.use(cookieParser()) // 解析客户端cookie并挂载到request里面

app.disable('x-powered-by')

app.get('/',(request,response)=>{
  response.send('holle wolrd')
})

// 种一个cookie
app.get('/test', (request, response) => {
  // 当用户请求/test时为用户种下一个cookie
  // 在express 给客户端种下cookie时是不需要使用任何第三方库的

  // 给客户端种下会话cookie
  // response.cookie('dom',1223)

  // 给客户端种下持久化cookie
  response.cookie('demo',1234,{maxAge:30*1000})
  response.send('<h1>我为你种下了cookie</h1>')
})

// 获取cookie
app.get('/test2', (request, response) => {
  // 当访问test2时，会获取到浏览器携带过来的cookie
  // 在express中更普遍的获取客户端携带过来的cookie，要借助一个中间件，名字: cookie-parser

  console.log(request.cookies)
  response.send('<h1>服务端拿到了cookie</h1>')
})

// 删除cookie
app.get('/test3', (request, response) => {
  // 第一种删除cookie方式
  // response.cookie('demo','',{maxAge:0})
  // 第二种删除cookie方式
  response.clearCookie('demo')
  response.send('<h1>删除cookie</h1>')
})
const port = 3636;

app.listen(port,err => {
  if(err) throw console.log(err)
  console.log(`服务器启动成功：http://localhost:${port}/`)
})
```



#### 1.添加cookie：

```js

const express = require('express')

const app = express()

app.disable('x-powered-by')

app.get('/',(request,response)=>{
  response.send('holle wolrd')
})

app.get('/test', (request, response) => {
  // 当用户请求/test时为用户种下一个cookie
  // 在express中 给客户端种一个cookie，不用借助任何第三方库
    
  // 给客户端种下会话cookie
  response.cookie('dom',1223)
  // 给客户端种下持久化cookie
  response.cookie('demo',123,{ maxAge:30*1000 })
  response.send('<h1>我为你种下了cookie</h1>')
})

const port = 3636;

app.listen(port,err => {
  if(err) throw console.log(err)
  console.log(`服务器启动成功：http://localhost:${port}/`)
})
```

#### 2.获取cookie：

```js
`读取客户端cookie需借助第三方库：   yarn add cookie-parser`

const express = require('express')

const app = express()

const cookieParser = require('cookie-parser') // 解析客户端cookie并挂载到request.cookies里面

app.use(cookieParser())

app.disable('x-powered-by')

app.get('/',(request,response)=>{
  response.send('holle wolrd')
})

app.get('/test2', (request, response) => {
  // 当访问test2时，会获取浏览器携带过来的cookie
  // 在express中更普遍的获取客户端携带过来的cookie，要借助一个中间件，名字: cookie-parser

  console.log(request.cookies)
  response.send('<h1>服务端拿到了cookie</h1>')
})

const port = 3636;

app.listen(port,err => {
  if(err) throw console.log(err)
  console.log(`服务器启动成功：http://localhost:${port}/`)
})
```

#### 3.删除cookie：

```js
 
const express = require('express')

const app = express()

app.disable('x-powered-by')

app.get('/',(request,response)=>{
  response.send('holle wolrd')
})

app.get('/test', (request, response) => {
    
  // 第一种删除cookie方式
  response.cookie('demo','',{maxAge:0})
  // 第二种删除cookie方式
  response.clearCookie('demo')
    
  response.send('<h1>删除cookie</h1>')
})

const port = 3636;

app.listen(port,err => {
  if(err) throw console.log(err)
  console.log(`服务器启动成功：http://localhost:${port}/`)
})
```



# session：

```js
1.下载安装: npm i express-session --save 用于在express中操作session

2.下载安装: npm i connect-mongo --save 用于将session写入数据库(session持久化)
	2.1.安装这个版本 npm i connect-mongo@3.2.0 --save
  
3.引入express-session模块:
	const session = require( ' express-session ' );
4.引入connect-mongo模块:
	const MongoStore = require( ' connect-mongo ' )(session);
5.编写全局配置对象:
app.use(session({
  name: 'userid', //设置cookie的name，默认值是:connect.sid
  secret: 'atguigu',//参与加密的字符串(又称签名)
  saveUninitialized: false, //是否在存储内容之前创建会话
  resave: true, //是否在每次请求时，强制重新保存session，即使他们没有变化
  store: new MongoStore({
    url: 'mongodb://localhost:27017/cookies_container',
    touchAfter: 24 * 3600//修改频率（例://在24小时之内只更新一次)
  }),
  cookie: {
    httponly: true,//开启后前端无法通过JS操作
    cookiemaxAge: 1000 * 30 //设置cookie的过期时间
  },
}));


```

# 1.数据加密sha1：

```js
yarn add sha1
const sha1 = require('sha1')
password:sha1(password)
```

# 2.数据加密MD5：

```js
yarn add md5
const md5 = require('md5')
password:md5(password)
```

# 原生AJAX-GET：

```js
1.实例化一个XMLHttpRequest对象。
2.给该对象绑定一个事件监听，名称为: onreadystatechange.
3.指定发请求的:方式、地址、参数。
4.发送请求。

`xhr内部有5种状态(readyState)`:
-0:xhr被实例化出来，状态就是0，即:初始化状态。
-1:请求还没有发出去，即: send方法还没有被调用，依然可以修改请求头。
-2:请求已经发出去了，即: send方法已经被调用了，不能再修改请求头，响应首行和响应头已经回来了。
-3:数据回来了(但是数据可能不完整，如果数据小，会在此阶段直接接收完毕，数据大有待于进一步接收)
-4:数据完全回来了
// 1.实例化一个XMLHttpRequest对象。
let xhr = new XMLHttpRequest()

// 2.给该对象绑定一个事件监听，名称为: onreadystatechange.
xhr.onreadystatechange = function () {
  if (xhr.readyState === 4 && xhr.status === 200) {
    console.log(xhr.response)
    data.innerHTML = xhr.response

  }
}

// 3.指定发请求的: 方式、地址、参数。
xhr.open('GET', `${url}?name=浪白&age=15`)

// 4.发送请求。
xhr.send()
```

# 原生AJAX-POST：

```js
`前端代码（重点）：
	xhr.setRequestHeader('content-type','application/x-www-form-urlencoded')	// 设置请求头

 服务端代码（重点）：
    // 解析post请求体中以urlencoded形式编码参数
    app.use(express.urlencoded({extended:true}))
		// 拿post请求参数
		console.log(request.body)
`

// 1.实例化一个XMLHttpRequest对象。
let xhr = new XMLHttpRequest()

// 2.给该对象绑定一个事件监听，名称为: onreadystatechange.
xhr.onreadystatechange = function () {
    if (xhr.readyState === 4 && xhr.status === 200) {
    console.log(xhr.response)
    data.innerHTML = xhr.response
  }
}

// 3.指定发请求的: 方式、地址、参数。
xhr.open('POST', `${url}test_post`)

// 设置post持有请求头（不设置的话服务端拿不到传过去的数据）
xhr.setRequestHeader('content-type','application/x-www-form-urlencoded')

// 4.发送请求。
xhr.send('name=浪白&age=147')
```

# AJAX：

```js

```

