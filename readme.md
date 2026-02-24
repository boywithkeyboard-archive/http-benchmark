## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `125019` | `6356` | `131974` |
| **79%** | [Hyper Express](#hyper-express) | `98279` | `6974` | `101889` |
| **31%** | [Node (Default)](#node-default) | `38213` | `12663` | `125622` |
| **29%** | [Fastify](#fastify) | `35954` | `9038` | `55931` |
| **26%** | [Koa](#koa) | `32614` | `16602` | `121050` |
| **26%** | [Hono](#hono) | `32170` | `7288` | `48440` |
| **10%** | [Carbon](#carbon) | `12173` | `2304` | `16251` |
| **7%** | [Express](#express) | `8746` | `1691` | `11948` |


### In Detail

- #### Carbon
  [NPM](https://npmjs.com/@sinclair/carbon) | [GitHub](https://github.com/sinclairzx81/carbon)
  ```js
  import { listen } from '@sinclair/carbon/http'

  listen({
    hostname: '127.0.0.1',
    port: 3000
  }, () => {
    return new Response('Hello World', {
      status: 200,
      headers: {
        'content-type': 'text/plain'
      }
    })
  })
  ```

  ```
  Statistics        Avg      Stdev        Max
    Reqs/sec     13540.78   10348.28  131471.72
    Latency        3.67ms     3.67ms   315.04ms
    HTTP codes:
      1xx - 0, 2xx - 89976, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10024
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10024
    Throughput:     2.78MB/s
  ```

- #### Express
  [NPM](https://npmjs.com/express) | [GitHub](https://github.com/expressjs/express)
  ```js
  import express from 'express'

  const app = express()

  app.get('/', function (req, res) {
    res.send('Hello World')
  })

  app.listen(3000)
  ```

  ```
  Statistics        Avg      Stdev        Max
    Reqs/sec      9894.51   10285.13  112168.43
    Latency        5.03ms     3.39ms   309.62ms
    HTTP codes:
      1xx - 0, 2xx - 86779, 3xx - 0, 4xx - 0, 5xx - 0
      others - 13221
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 13209
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 12
    Throughput:     2.47MB/s
  ```

- #### Fastify
  [NPM](https://npmjs.com/fastify) | [GitHub](https://github.com/fastify/fastify)
  ```js
  import fastify from 'fastify'

  const app = fastify({
    logger: false
  })

  app.get('/', (req, res) => {
    res.send('Hello World')
  })

  app.listen({ port: 3000 }, (err) => {
    if (err) throw err
  })
  ```

  ```
  Statistics        Avg      Stdev        Max
    Reqs/sec     34403.09    8146.94   54879.80
    Latency        1.45ms     1.49ms   132.00ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.80MB/s
  ```

- #### Hono
  [NPM](https://npmjs.com/hono) | [GitHub](https://github.com/honojs/hono)
  ```js
  import { serve } from '@hono/node-server'
  import { Hono } from 'hono'

  const app = new Hono()

  app.get('/', (c) => c.text('Hello World'))

  serve(app)
  ```

  ```
  Statistics        Avg      Stdev        Max
    Reqs/sec     31252.10    7409.31   48396.14
    Latency        1.60ms     1.60ms   139.63ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.05MB/s
  ```

- #### Hyper Express
  [NPM](https://npmjs.com/hyper-express) | [GitHub](https://github.com/kartikk221/hyper-express)
  ```js
  import HyperExpress from 'hyper-express'

  const server = new HyperExpress.Server()

  server.get('/', (req, res) => {
    res.send('Hello World')
  })

  server.listen(3000)
  ```

  ```
  Statistics        Avg      Stdev        Max
    Reqs/sec    100386.80    6998.37  104179.06
    Latency      496.34us    43.35us     3.91ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    14.25MB/s
  ```

- #### Koa
  [NPM](https://npmjs.com/koa) | [GitHub](https://github.com/koajs/koa)
  ```js
  import Koa from 'koa'

  const app = new Koa()

  app.use(ctx => {
    ctx.body = 'Hello World'
  })

  app.listen(3000)
  ```

  ```
  Statistics        Avg      Stdev        Max
    Reqs/sec     32201.43   18439.36  122186.15
    Latency        1.55ms     1.77ms   157.62ms
    HTTP codes:
      1xx - 0, 2xx - 85620, 3xx - 0, 4xx - 0, 5xx - 0
      others - 14380
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 14380
    Throughput:     6.23MB/s
  ```

- #### Node (Default)
  [Website](https://nodejs.org/api/http.html)
  ```js
  import { createServer } from 'node:http'

  const server = createServer((req, res) => {
    res.writeHead(200, {
      'content-type': 'text/plain'
    })

    res.write('Hello World')

    res.end()
  })

  server.listen(3000, '127.0.0.1')
  ```

  ```
  Statistics        Avg      Stdev        Max
    Reqs/sec     38120.57   11337.31  103738.56
    Latency        1.31ms     1.37ms   115.96ms
    HTTP codes:
      1xx - 0, 2xx - 95972, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4028
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4028
    Throughput:     8.37MB/s
  ```

- #### uWS
  [GitHub](https://github.com/uNetworking/uWebSockets.js)
  ```js
  import { App } from 'uWebSockets.js'

  const app = App()

  app.get('/', (res, req) => {
    res.end('Hello World')
  })

  app.listen(3000, () => {})
  ```

  ```
  Statistics        Avg      Stdev        Max
    Reqs/sec    125409.00    5563.03  134964.99
    Latency      396.07us   113.32us     7.05ms
    HTTP codes:
      1xx - 0, 2xx - 95912, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4088
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4088
    Throughput:    19.04MB/s
  ```


