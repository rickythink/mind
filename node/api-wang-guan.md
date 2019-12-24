# API 网关

> [https://segmentfault.com/a/1190000010669382](https://segmentfault.com/a/1190000010669382)

在 Node.js 中，您可以使用 [http-proxy](https://www.npmjs.com/package/http-proxy) 软件包简单地代理对特定服务的请求，也可以使用更多丰富功能的 [express-gateway](http://www.express-gateway.io/) 来创建 API 网关。

在我们的第一个 API 网关示例中，我们在将代码委托给 **user** 服务之前验证请求。

```javascript
    const express = require('express')
    const httpProxy = require('express-http-proxy')
    const app = express()

    const userServiceProxy = httpProxy('https://user-service')

    // 身份认证
    app.use((req, res, next) => {
      // TODO: 身份认证逻辑
      next()
    })

    // 代理请求
    app.get('/users/:userId', (req, res, next) => {
      userServiceProxy(req, res, next)
    })
```

另一种示例可能是在您的 API 网关中发出新的请求，并将响应返回给客户端：

```javascript
    const express = require('express')
    const request = require('request-promise-native')
    const app = express()

    // 解决: GET /users/me
    app.get('/users/me', async (req, res) => {
      const userId = req.session.userId
      const uri = `https://user-service/users/${userId}`
      const user = await request(uri)
      res.json(user)
    })
```

