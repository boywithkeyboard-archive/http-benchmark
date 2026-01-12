## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `123863` | `5152` | `133540` |
| **82%** | [Hyper Express](#hyper-express) | `101013` | `6889` | `104628` |
| **31%** | [Node (Default)](#node-default) | `38582` | `11022` | `102434` |
| **30%** | [Fastify](#fastify) | `37364` | `8266` | `53167` |
| **27%** | [Hono](#hono) | `33380` | `7442` | `47154` |
| **27%** | [Koa](#koa) | `33148` | `15182` | `117659` |
| **10%** | [Carbon](#carbon) | `12249` | `2657` | `18213` |
| **7%** | [Express](#express) | `8972` | `1704` | `11943` |


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
    Reqs/sec     13881.75   12086.77  120325.70
    Latency        3.59ms     3.67ms   314.15ms
    HTTP codes:
      1xx - 0, 2xx - 86555, 3xx - 0, 4xx - 0, 5xx - 0
      others - 13445
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 13445
    Throughput:     2.73MB/s
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
    Reqs/sec      9932.86   11204.98  110774.09
    Latency        5.02ms     3.44ms   310.43ms
    HTTP codes:
      1xx - 0, 2xx - 85409, 3xx - 0, 4xx - 0, 5xx - 0
      others - 14591
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 14576
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 15
    Throughput:     2.43MB/s
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
    Reqs/sec     35315.28    7728.26   52358.34
    Latency        1.41ms     1.44ms   125.02ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.01MB/s
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
    Reqs/sec     32240.97    7017.34   48393.09
    Latency        1.55ms     1.55ms   136.04ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.28MB/s
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
    Reqs/sec     99736.64    6663.38  105386.16
    Latency      499.56us    50.59us     2.11ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    14.17MB/s
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
    Reqs/sec     32459.98   15092.87  106907.22
    Latency        1.54ms     1.79ms   153.37ms
    HTTP codes:
      1xx - 0, 2xx - 89505, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10495
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10495
    Throughput:     6.56MB/s
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
    Reqs/sec     38784.27   10569.89  104767.55
    Latency        1.29ms     1.37ms   111.89ms
    HTTP codes:
      1xx - 0, 2xx - 96158, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3842
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3842
    Throughput:     8.53MB/s
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
    Reqs/sec    124651.42    7884.32  143862.99
    Latency      399.15us   156.97us     8.15ms
    HTTP codes:
      1xx - 0, 2xx - 95742, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4258
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4258
    Throughput:    18.83MB/s
  ```


