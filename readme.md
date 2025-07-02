## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72461` | `4668` | `83527` |
| **84%** | [Hyper Express](#hyper-express) | `61007` | `3309` | `63693` |
| **34%** | [Node (Default)](#node-default) | `24690` | `7201` | `52893` |
| **34%** | [Fastify](#fastify) | `24384` | `7621` | `36221` |
| **28%** | [Hono](#hono) | `20091` | `5699` | `28632` |
| **25%** | [Koa](#koa) | `18108` | `6822` | `54156` |
| **11%** | [Carbon](#carbon) | `8179` | `1370` | `10293` |
| **9%** | [Express](#express) | `6347` | `1077` | `8416` |


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
    Reqs/sec      8764.52    4905.04   56575.93
    Latency        5.70ms     4.49ms   385.86ms
    HTTP codes:
      1xx - 0, 2xx - 92527, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7473
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7473
    Throughput:     1.84MB/s
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
    Reqs/sec      6205.95     980.00    8400.01
    Latency        8.05ms     3.57ms   349.74ms
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
    Reqs/sec     22947.98    6349.42   35757.07
    Latency        2.18ms     1.99ms   180.42ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.21MB/s
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
    Reqs/sec     20651.46    5564.81   28500.78
    Latency        2.42ms     2.07ms   187.53ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.67MB/s
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
    Reqs/sec     61082.73    3156.04   64292.01
    Latency      816.24us    66.93us     2.60ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.68MB/s
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
    Reqs/sec     18253.12    6932.01   53850.36
    Latency        2.73ms     2.43ms   212.19ms
    HTTP codes:
      1xx - 0, 2xx - 93808, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6192
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6192
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
    Reqs/sec     25636.40    7860.85   53518.39
    Latency        1.95ms     1.96ms   167.08ms
    HTTP codes:
      1xx - 0, 2xx - 96739, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3261
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3261
    Throughput:     5.67MB/s
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
    Reqs/sec     73548.98    4646.80   84588.45
    Latency      677.67us   185.02us    10.34ms
    HTTP codes:
      1xx - 0, 2xx - 96570, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3430
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3430
    Throughput:    11.21MB/s
  ```


