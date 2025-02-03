## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72718` | `5288` | `79734` |
| **85%** | [Hyper Express](#hyper-express) | `61727` | `2750` | `64629` |
| **36%** | [Node (Default)](#node-default) | `25958` | `7673` | `58024` |
| **34%** | [Fastify](#fastify) | `24372` | `7703` | `35719` |
| **29%** | [Hono](#hono) | `20827` | `6394` | `29178` |
| **26%** | [Koa](#koa) | `19161` | `7673` | `58041` |
| **11%** | [Carbon](#carbon) | `7847` | `1350` | `10033` |
| **8%** | [Express](#express) | `6049` | `947` | `7891` |


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
    Reqs/sec      8534.07    4192.79   49438.42
    Latency        5.85ms     4.79ms   414.41ms
    HTTP codes:
      1xx - 0, 2xx - 92489, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7511
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7511
    Throughput:     1.79MB/s
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
    Reqs/sec      6228.20    1004.99    8156.46
    Latency        8.02ms     3.76ms   359.80ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.78MB/s
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
    Reqs/sec     23816.81    7571.60   35243.79
    Latency        2.10ms     2.13ms   192.34ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.41MB/s
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
    Reqs/sec     21401.84    6164.74   29120.30
    Latency        2.33ms     2.13ms   191.40ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.83MB/s
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
    Reqs/sec     61749.56    3928.28   68932.34
    Latency      807.56us    76.35us     3.00ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.77MB/s
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
    Reqs/sec     19088.85    6981.06   54971.66
    Latency        2.61ms     2.42ms   211.39ms
    HTTP codes:
      1xx - 0, 2xx - 93432, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6568
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6568
    Throughput:     4.03MB/s
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
    Reqs/sec     26081.26    7760.91   50021.93
    Latency        1.91ms     1.96ms   168.90ms
    HTTP codes:
      1xx - 0, 2xx - 97950, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2050
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2050
    Throughput:     5.85MB/s
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
    Reqs/sec     74432.19    5741.37   95367.07
    Latency      668.49us   163.22us     7.99ms
    HTTP codes:
      1xx - 0, 2xx - 97472, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2528
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2528
    Throughput:    11.49MB/s
  ```


