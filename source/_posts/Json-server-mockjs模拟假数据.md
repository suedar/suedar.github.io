---
title: Json-server + mockjs模拟假数据
date: 2019-08-11 16:02:54
tags:
---

鉴于这个代码开源出去感觉会被打，还是写个博客吧。

其实操作很简单，只要把文件读取，通过`mock`转化，再导出文件，然后在`json-server`填写相应路径即可。

源文件`test.json`
```json
{
    "list|1-10": [{
        "id|+1": 1
    }],
    "user": {
        "name": "@name",
        "id": "@id"
    }
}
```

通过`mockjs`处理

``` js
const Mock = require('mockjs');
const fs = require("fs");

let data = fs.readFileSync("test.json", "utf-8");

data = data.toString();
data = JSON.parse(data)
data = Mock.mock(data);
data = JSON.stringify(data);

fs.writeFile('res.json', data, function(err){
    if(err){
        console.error(err);
    }
})

```

得`{"list":[{"id":1},{"id":2},{"id":3}],"user":{"name":"John Martinez","id":"320000199809111043"}}`


完整`json-server`配置文件

``` js

// server.js
const path = require('path');
const fs = require("fs");
const Mock = require('mockjs');
const jsonServer = require('json-server')

let data = fs.readFileSync("test.json", "utf-8");

data = data.toString();
data = JSON.parse(data)
data = Mock.mock(data);
data = JSON.stringify(data);

fs.writeFile('res.json', data, function(err) {
    if(err){
        console.error(err);
    }
    const server = jsonServer.create()
    const router = jsonServer.router(path.join(__dirname, 'res.json'))
    const middlewares = jsonServer.defaults()

    // Set default middlewares (logger, static, cors and no-cache)
    server.use(middlewares)

    // Add custom routes before JSON Server router
    server.get('/echo', (req, res) => {
        res.jsonp(req.query)
    })

    // To handle POST, PUT and PATCH you need to use a body-parser
    // You can use the one used by JSON Server
    server.use(jsonServer.bodyParser)
    server.use((req, res, next) => {
        if (req.method === 'POST') {
            req.body.createdAt = Date.now()
        }
        // Continue to JSON Server router
        next()
    })

    // Use default router
    server.use(router)
    server.listen(3000, () => {
        console.log('JSON Server is running')
    })

})
```

打开localhost:3000
