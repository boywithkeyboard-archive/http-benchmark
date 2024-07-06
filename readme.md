## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `76225` | `6213` | `86298` |
| **85%** | [Hyper Express](#hyper-express) | `64498` | `4209` | `69296` |
| **34%** | [Node (Default)](#node-default) | `25910` | `8707` | `62597` |
| **31%** | [Fastify](#fastify) | `23618` | `7144` | `35831` |
| **26%** | [Hono](#hono) | `19820` | `4966` | `29816` |
| **24%** | [Koa](#koa) | `17918` | `6346` | `51716` |
| **11%** | [Carbon](#carbon) | `8101` | `1369` | `10746` |
| **9%** | [Express](#express) | `6627` | `1094` | `8504` |


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
    Reqs/sec      8644.24    5088.65   63229.83
    Latency        5.78ms     4.16ms   357.26ms
    HTTP codes:
      1xx - 0, 2xx - 92206, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7794
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7794
    Throughput:     1.81MB/s
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
    Reqs/sec      6625.28    1049.86    8530.56
    Latency        7.54ms     3.56ms   339.96ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.90MB/s
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
    Reqs/sec     24542.14    7861.00   36686.64
    Latency        2.04ms     1.95ms   175.44ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.56MB/s
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
    Reqs/sec     21036.75    5865.24   30715.54
    Latency        2.38ms     2.01ms   181.26ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.75MB/s
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
    Reqs/sec     63752.12    3453.64   68150.09
    Latency      782.38us    66.44us     3.67ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.05MB/s
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
    Reqs/sec     18259.25    6656.00   48715.07
    Latency        2.73ms     2.40ms   210.27ms
    HTTP codes:
      1xx - 0, 2xx - 93628, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6372
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6372
    Throughput:     3.87MB/s
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
    Reqs/sec     27238.85    8507.74   61049.50
    Latency        1.83ms     1.88ms   158.07ms
    HTTP codes:
      1xx - 0, 2xx - 96987, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3013
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3012
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 1
    Throughput:     6.04MB/s
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
    Reqs/sec     76190.24    8636.54   91829.57
    Latency      656.00us   270.42us    24.55ms
    HTTP codes:
      1xx - 0, 2xx - 97577, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2423
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2422
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 1
    Throughput:    11.73MB/s
  ```


