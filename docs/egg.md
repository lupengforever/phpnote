## 初识

### egg 环境搭建入门

> `windows` `node`

直接使用脚手架,快速搭建 nnn

`$` mkdir egg-example && cd egg-example

`$` npm init egg --type=simple

`$` npm i

启动项目：

`$` npm run dev

`$` http://127.0.0.1:7001

### 基础认知

#### 目录结构

[目录结构参见 here](https://eggjs.org/zh-cn/basics/structure.html)

#### 路由（Router）

!>egg 路由统一在`app/router.js`文件配置[官方文档地址](https://eggjs.org/zh-cn/basics/router.html)

```js
module.exports = app => {
  const { router, controller } = app;
  router.get("/user/:id", controller.user.info);
};
```

app/controller 目录下面实现 Controller

```js
// app/controller/user.js
class UserController extends Controller {
  async info() {
    const { ctx } = this;
    ctx.body = {
      name: `hello ${ctx.params.id}`
    };
  }
}
```

这样就完成了一个最简单的 Router 定义，当用户执行 GET /user/123，user.js 这个里面的 info 方法就会执行。

> RESTful app.resources('routerName', 'pathMatch', controller) 快速在一个路径上生成 CRUD 路由结构

| Method | Path            | Route Name | Controller.Action             |
| ------ | --------------- | ---------- | ----------------------------- |
| GET    | /posts          | posts      | app.controllers.posts.index   |
| GET    | /posts/new      | new_post   | app.controllers.posts.new     |
| GET    | /posts/:id      | post       | app.controllers.posts.show    |
| GET    | /posts/:id/edit | edit_post  | app.controllers.posts.edit    |
| POST   | /posts          | posts      | app.controllers.posts.create  |
| PUT    | /posts/:id      | post       | app.controllers.posts.update  |
| DELETE | /posts/:id      | post       | app.controllers.posts.destory |

```js
exports.index = async () => {};
exports.new = async () => {};
exports.create = async () => {};
exports.show = async () => {};
exports.edit = async () => {};
exports.update = async () => {};
exports.destroy = async () => {};
```

##### 参数获取

###### Query String 方式

```js
// app/router.js
module.exports = app => {
  app.router.get("/search", app.controller.search.index);
};

// app/controller/search.js
exports.index = async ctx => {
  ctx.body = `search: ${ctx.query.name}`;
};

// curl http://127.0.0.1:7001/search?name=egg
```

###### 参数命名方式（url）

```js
// app/router.js
module.exports = app => {
  app.router.get("/user/:id/:name", app.controller.user.info);
};

// app/controller/user.js
exports.info = async ctx => {
  ctx.body = `user: ${ctx.params.id}, ${ctx.params.name}`;
};

// curl http://127.0.0.1:7001/user/123/xiaoming
```

###### 正则

```js
// app/router.js
module.exports = app => {
  app.router.get(
    /^\/package\/([\w-.]+\/[\w-.]+)\$/,
    app.controller.package.detail
  );
};

// app/controller/package.js
exports.detail = async ctx => {
  // 如果请求 URL 被正则匹配， 可以按照捕获分组的顺序，从 ctx.params 中获取。
  // 按照下面的用户请求，`ctx.params[0]` 的 内容就是 `egg/1.0.0`
  ctx.body = `package:${ctx.params[0]}`;
};

// curl http://127.0.0.1:7001/package/egg/1.0.0
```

###### 表单

```js
// app/router.js
module.exports = app => {
  app.router.post("/form", app.controller.form.post);
};

// app/controller/form.js
exports.post = async ctx => {
  ctx.body = `body: ${JSON.stringify(ctx.request.body)}`;
};
```

#### 控制器

router 分发请求到对应控制器 控制器解析用户的输入返回处理后的数据 资源路由分发到对应控制的方法（调用 service 处理业务逻辑），基本如下：

     http 带参请求 -> controller 校验数据 -> service 处理业务逻辑之后返回 -> 控制器 -> http 响应

[controller 定义]()

```js
// app/controller/post.js
const Controller = require("egg").Controller;
class PostController extends Controller {
  async create() {
    const { ctx, service } = this;
    const createRule = {
      title: { type: "string" },
      content: { type: "string" }
    };
    // 校验参数
    ctx.validate(createRule);
    // 组装参数
    const author = ctx.session.userId;
    const req = Object.assign(ctx.request.body, { author });
    // 调用 Service 进行业务处理
    const res = await service.post.create(req);
    // 设置响应内容和响应状态码
    ctx.body = { id: res.id };
    ctx.status = 201;
  }
}
module.exports = PostController;
```

`路由内可访问 // router.get('/post',controller.post.create)`
