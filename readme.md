## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `76829` | `6161` | `87471` |
| **82%** | [Hyper Express](#hyper-express) | `62987` | `3884` | `65591` |
| **32%** | [Node (Default)](#node-default) | `24820` | `7082` | `50306` |
| **30%** | [Fastify](#fastify) | `22758` | `6518` | `35617` |
| **28%** | [Hono](#hono) | `21343` | `6092` | `28880` |
| **23%** | [Koa](#koa) | `17486` | `6002` | `56741` |
| **11%** | [Carbon](#carbon) | `8121` | `1375` | `10354` |
| **8%** | [Express](#express) | `6469` | `1038` | `8357` |


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
    Reqs/sec      8506.68    3327.23   53031.86
    Latency        5.86ms     4.19ms   359.87ms
    HTTP codes:
      1xx - 0, 2xx - 95864, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4136
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4136
    Throughput:     1.85MB/s
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
    Reqs/sec      6383.44     965.24    8315.20
    Latency        7.83ms     3.64ms   347.26ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     22981.85    6741.77   34043.34
    Latency        2.17ms     1.95ms   177.37ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.22MB/s
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
    Reqs/sec     20043.00    5590.75   28692.49
    Latency        2.49ms     2.02ms   181.59ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.53MB/s
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
    Reqs/sec     62587.81    3529.06   66127.57
    Latency      796.75us    76.72us     2.83ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.89MB/s
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
    Reqs/sec     18311.12    6393.75   57728.55
    Latency        2.73ms     2.31ms   205.33ms
    HTTP codes:
      1xx - 0, 2xx - 94127, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5873
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5873
    Throughput:     3.90MB/s
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
    Reqs/sec     25709.66    8136.65   58908.11
    Latency        1.94ms     1.95ms   166.59ms
    HTTP codes:
      1xx - 0, 2xx - 96137, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3863
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3863
    Throughput:     5.66MB/s
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
    Reqs/sec     74881.01    6491.84   87596.64
    Latency      664.19us   194.45us    13.71ms
    HTTP codes:
      1xx - 0, 2xx - 96530, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3470
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3470
    Throughput:    11.44MB/s
  ```


