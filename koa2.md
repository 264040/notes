# koa2

```js
yarn add koa
npm install koa
yarn add koa-generator
npm install -g koa-generator

const koa = require('koa');
const app = new koa();

app.use( ctx => {
	ctx.body = 'Hello koa'
});

app.listen(3000,() => {
	console.log('服务器启动成功')
})
```

# koa2

```js
koa2 name
```

# koa2中间件

```js
const Koa = require('koa');
const app = new Koa();

app.use( async (ctx, next) => {
  console.info(1)
  await next();
  ctx.body = '<h1>Hello world!</h1>'
});

app.use(async (ctx, next) => {
  console.info(2)
  await next()
})
app.use( (ctx, next) => {
  console.info(3)
})

const listens = 3333;
app.listen(listens, () => {
  console.info(`Koa服务器启动成功：http://localhost:${listens}`)
}) 
```

# koa2-Router(原生写法)

```js
const Koa = require('koa');
const app = new Koa();

app.use(async (ctx, next) => {
  await (() => {
    if (ctx.url === '/') {
      ctx.body = '首页'
    } else if (ctx.url === '/user') {

      if (ctx.method === 'GET') {
        ctx.body = '用户列表'
      } else if (ctx.method === 'POST') {
        ctx.body = '添加用户'
      } 
      
    } else {
      ctx.body = '路径错误'
      ctx.status = 404
    }
  })()
});

const listens = 3333;
app.listen(listens, () => {
  console.info(`Koa服务器启动成功：http://localhost:${listens}`)
})
```

# koa2-Router(koa-router)

```js
yarn add koa-router -S
npm install koa-router --save

const koa = require('koa');
const app = new koa();
const Router = require('koa-router');
const router = new Router({
  prefix: '/user'
});
const ports = 6565;

router.get('/', async ( ctx ) => {
  ctx.body = '首页';
});

router.get('/list', ( ctx ) => {
 	ctx.body = '商品列表';
});

router.post('/list', ( ctx ) => {
  ctx.body = '添加商品';
});

app.use(router.routes());
 
app.listen(port, () => {
  console.log(`服务器启动成功 http://localhost:port/`)
})
```

# koa2-获取get请求参数

```js
routers.get('/list', async( ctx ) => {
  ctx.body = '商品列表'
  console.info(ctx.query)
})
```

# koa2-获取post请求参数

```js
`获取post请求的参数需要安装第三方包`
yarn add koa-bodyparser -S
npm install koa-bodyparser --save/-s

const koa = require('koa');
const app = new koa();
const Router = require('koa-router');
const router = new Router({
  prefix: '/paths'
});
const bodyparser = require('koa-bodyparser');

routers.post('/list', async( ctx ) => {
  ctx.body = '商品列表'
  console.info(ctx.request.body)
});

app.use(bodyparser());
app.use(router.routes());

app.listen(3000,() => {
  console.info('服务器启动成功')
})
```

# koa2-获取路由参数

```js
routers.get('/listids/:id', async( ctx ) => { 
  ctx.body = '路由参数';
  // http://localhost:3333/user/listids/牛啊
  console.info(ctx.params.id) // id => 牛啊
});
```

# 增删改查小案例

```js
const Koa = require('koa');
const app = new Koa();
const router = require('koa-router');
const bodyparser = require('koa-bodyparser');
const routers = new router({
  prefix: '/user'
});

const listData = [{"userName":"LEW","userPsw":"123456"}]

// czh 查询
routers.get('/', async ( ctx ) => {
  ctx.body = {
    data: listData
  }
});

routers.get('/list', async( ctx ) => {
  ctx.body = '商品列表'
  console.info(ctx.query)
});

// czh 添加
routers.post('/list', async( ctx ) => {
  const { userName, userPsw } = ctx.request.body; 
  listData.push({
    userName,
    userPsw
  })
  ctx.body = {
    code: "200",
    massage: '添加成功'
  }
  console.info('数据添加成功',userName,userPsw)
});

// czh 删除
routers.post('/delid', async(ctx) => {
  const { id } = ctx.request.body;
  if(Number(id) > (listData.length - 1)){
    ctx.throw(404,'查无结果')
  }
  listData.splice(Number(id),1);
  ctx.body = {
    code: "200",
    massage: '删除成功'
  }
})

// czh 修改
routers.post('/updata', async(ctx) => {
  let {  userName, userPsw, id } = ctx.request.body;
  const updata = listData.splice(id,1,{
    userName,
    userPsw
  })
  ctx.body = {
    code: '200',
    massage: {
      data:listData
    }
  }
console.info(updata)
})

// czh 路由查询
routers.get('/listids/:id', async( ctx ) => {
  let { id } = ctx.params; 
  ctx.body = {
    code:'200',
    massage: '路由参数查询',
    data:listData[Number(id)]
  } 
});


app.use(bodyparser());
app.use(routers.routes());

const listens = 33335;
app.listen(listens, () => {
  console.info(`Koa服务器启动成功：http://localhost:${listens}`)
})
```

# 处理异常の中间件(koa-json-error)

```js
yarn add koa-json-error -S
npm install koa-json-error --save

const koa = require('koa'); 
const app = new koa();
const jsonerror = require('koa-json-error');

app.use(jsonerror());


```

# 校验请求参数(koa-parameter)

```js
yarn add koa-parameter -S
npm install koa-parameter --save

const koa = require('koa');
const app = new koa();
const Router = require('koa-router');
const bodyparser = require('koa-bodyparser');
const parameter = require('koa-parameter');

// czh 添加
routers.post('/list', async (ctx) => {
  // 参数校验规则
  ctx.verifyParams({
    userName: {
      type: 'string',
      required: true
    },
    userPsw: {
      type: 'string',
      required: true
    }
  })
  const { userName, userPsw } = ctx.request.body;
  listData.push({
    userName,
    userPsw
  })
  ctx.body = {
    code: "200",
    message: '添加成功'
  } 
});

app.use(bodyparser());
app.use(parameter(app));
app.use(routers.routes());

const listens = 33335;
app.listen(listens, () => {
  console.info(`Koa服务器启动成功：http://localhost:${listens}`)
})
```

#  JWT(josnwebtoken)

```js
yarn add jsonwebtoken -S
npm install jsonwebtoken -save

yarn add koa-jwt -S
npm i koa-jwt --save
```

#  图片上传(koa-multer)

```js
yarn add koa-multer -S
npm install koa-multer --save

```

#  

```js

```

#  

```js

```

