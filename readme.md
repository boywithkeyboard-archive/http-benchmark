## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73532` | `7004` | `101194` |
| **85%** | [Hyper Express](#hyper-express) | `62601` | `3048` | `70929` |
| **36%** | [Node (Default)](#node-default) | `26633` | `7818` | `57550` |
| **32%** | [Fastify](#fastify) | `23893` | `6891` | `34968` |
| **29%** | [Hono](#hono) | `21203` | `5915` | `30013` |
| **26%** | [Koa](#koa) | `18957` | `7510` | `65851` |
| **11%** | [Carbon](#carbon) | `8079` | `1368` | `10370` |
| **9%** | [Express](#express) | `6325` | `993` | `8354` |


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
    Reqs/sec      8682.97    3510.23   51067.57
    Latency        5.75ms     4.23ms   363.75ms
    HTTP codes:
      1xx - 0, 2xx - 95329, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4671
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4671
    Throughput:     1.88MB/s
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
    Reqs/sec      6856.62    3942.74   59298.71
    Latency        7.28ms     3.92ms   358.24ms
    HTTP codes:
      1xx - 0, 2xx - 93046, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6954
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6954
    Throughput:     1.83MB/s
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
    Reqs/sec     25164.29    7877.94   35156.65
    Latency        1.98ms     2.04ms   180.41ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.71MB/s
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
    Reqs/sec     21020.44    6006.55   29307.28
    Latency        2.38ms     2.06ms   185.83ms
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
    Reqs/sec     62677.15    3421.87   70093.43
    Latency      795.44us    76.07us     4.57ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.90MB/s
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
    Reqs/sec     17883.04    6525.72   53297.82
    Latency        2.77ms     2.36ms   207.44ms
    HTTP codes:
      1xx - 0, 2xx - 93571, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6429
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6429
    Throughput:     3.81MB/s
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
    Reqs/sec     26693.56    7538.93   44054.10
    Latency        1.87ms     1.88ms   161.76ms
    HTTP codes:
      1xx - 0, 2xx - 97512, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2488
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2482
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 6
    Throughput:     5.96MB/s
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
    Reqs/sec     72212.05    5746.32   84043.92
    Latency      689.48us   229.14us    14.21ms
    HTTP codes:
      1xx - 0, 2xx - 96648, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3352
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3352
    Throughput:    11.04MB/s
  ```


