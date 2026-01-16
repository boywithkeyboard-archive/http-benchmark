## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `129005` | `4620` | `136312` |
| **82%** | [Hyper Express](#hyper-express) | `105268` | `7426` | `109304` |
| **32%** | [Node (Default)](#node-default) | `40817` | `12232` | `108592` |
| **30%** | [Fastify](#fastify) | `38295` | `9250` | `57989` |
| **27%** | [Koa](#koa) | `34649` | `16773` | `124009` |
| **26%** | [Hono](#hono) | `33313` | `7673` | `50678` |
| **10%** | [Carbon](#carbon) | `12805` | `2554` | `16876` |
| **7%** | [Express](#express) | `9018` | `1643` | `12170` |


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
    Reqs/sec     13929.00    9992.62  112641.15
    Latency        3.58ms     3.56ms   304.51ms
    HTTP codes:
      1xx - 0, 2xx - 90610, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9390
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9390
    Throughput:     2.87MB/s
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
    Reqs/sec     10554.45   11070.94  115609.17
    Latency        4.73ms     3.19ms   292.81ms
    HTTP codes:
      1xx - 0, 2xx - 86408, 3xx - 0, 4xx - 0, 5xx - 0
      others - 13592
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 13592
    Throughput:     2.61MB/s
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
    Reqs/sec     37135.57    8564.54   55312.05
    Latency        1.35ms     1.45ms   125.25ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.40MB/s
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
    Reqs/sec     33528.13    7796.00   48983.58
    Latency        1.49ms     1.47ms   128.21ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.57MB/s
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
    Reqs/sec    104884.78    7702.08  108837.69
    Latency      474.91us    44.00us     1.79ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    14.89MB/s
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
    Reqs/sec     32210.04   14518.16  111188.72
    Latency        1.55ms     1.72ms   147.66ms
    HTTP codes:
      1xx - 0, 2xx - 90365, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9635
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9635
    Throughput:     6.58MB/s
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
    Reqs/sec     40607.82   13134.53  124391.33
    Latency        1.23ms     1.34ms   112.63ms
    HTTP codes:
      1xx - 0, 2xx - 94211, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5789
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5789
    Throughput:     8.75MB/s
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
    Reqs/sec    128800.84    4751.22  135610.75
    Latency      385.61us   118.09us     5.66ms
    HTTP codes:
      1xx - 0, 2xx - 94498, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5502
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5502
    Throughput:    19.26MB/s
  ```


