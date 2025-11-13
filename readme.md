## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `124543` | `6162` | `134985` |
| **79%** | [Hyper Express](#hyper-express) | `98071` | `9151` | `104992` |
| **29%** | [Node (Default)](#node-default) | `36188` | `10617` | `102680` |
| **28%** | [Fastify](#fastify) | `35341` | `7839` | `53789` |
| **26%** | [Koa](#koa) | `32353` | `15643` | `118276` |
| **25%** | [Hono](#hono) | `31463` | `6410` | `42601` |
| **10%** | [Carbon](#carbon) | `11914` | `2289` | `15857` |
| **7%** | [Express](#express) | `8771` | `1675` | `11758` |


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
    Reqs/sec     12812.44    8729.30   98737.50
    Latency        3.89ms     3.83ms   327.60ms
    HTTP codes:
      1xx - 0, 2xx - 91314, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8686
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8686
    Throughput:     2.66MB/s
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
    Reqs/sec      9918.86   10401.45  107993.66
    Latency        5.03ms     3.47ms   314.45ms
    HTTP codes:
      1xx - 0, 2xx - 86647, 3xx - 0, 4xx - 0, 5xx - 0
      others - 13353
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 13339
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 14
    Throughput:     2.46MB/s
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
    Reqs/sec     40627.90   20702.34  110402.64
    Latency        1.22ms     1.52ms   129.08ms
    HTTP codes:
      1xx - 0, 2xx - 76477, 3xx - 0, 4xx - 0, 5xx - 0
      others - 23523
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 23521
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 2
    Throughput:     7.07MB/s
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
    Reqs/sec     34131.74   13639.90  108405.15
    Latency        1.46ms     1.59ms   134.78ms
    HTTP codes:
      1xx - 0, 2xx - 90792, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9208
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9208
    Throughput:     7.00MB/s
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
    Reqs/sec    100799.33    5784.09  111078.33
    Latency      493.51us   173.86us     5.98ms
    HTTP codes:
      1xx - 0, 2xx - 91549, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8451
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8451
    Throughput:    13.11MB/s
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
    Reqs/sec     30548.16   15129.40  115280.73
    Latency        1.63ms     1.82ms   153.66ms
    HTTP codes:
      1xx - 0, 2xx - 89083, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10917
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10917
    Throughput:     6.15MB/s
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
    Reqs/sec     35439.86    9230.85   98568.71
    Latency        1.41ms     1.37ms   113.54ms
    HTTP codes:
      1xx - 0, 2xx - 96531, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3469
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3469
    Throughput:     7.83MB/s
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
    Reqs/sec    125104.48    6434.12  133022.22
    Latency      396.89us   149.67us     9.04ms
    HTTP codes:
      1xx - 0, 2xx - 95778, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4222
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4222
    Throughput:    18.96MB/s
  ```


