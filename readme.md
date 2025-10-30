## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73508` | `3119` | `82683` |
| **83%** | [Hyper Express](#hyper-express) | `61220` | `2586` | `63748` |
| **33%** | [Node (Default)](#node-default) | `24354` | `7716` | `72217` |
| **32%** | [Fastify](#fastify) | `23312` | `7249` | `34863` |
| **28%** | [Hono](#hono) | `20505` | `5779` | `28779` |
| **26%** | [Koa](#koa) | `18904` | `7735` | `61681` |
| **11%** | [Carbon](#carbon) | `8141` | `1482` | `10449` |
| **8%** | [Express](#express) | `6243` | `1052` | `8362` |


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
    Reqs/sec      9000.81    7487.30   75180.31
    Latency        5.55ms     4.38ms   378.74ms
    HTTP codes:
      1xx - 0, 2xx - 88292, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11708
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11708
    Throughput:     1.80MB/s
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
    Reqs/sec      6211.43    1006.99    8146.58
    Latency        8.05ms     3.88ms   371.65ms
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
    Reqs/sec     22836.20    7058.52   34621.08
    Latency        2.19ms     2.12ms   190.39ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.18MB/s
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
    Reqs/sec     20432.06    5631.94   28623.97
    Latency        2.45ms     2.11ms   187.68ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.61MB/s
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
    Reqs/sec     61301.22    3515.79   65079.72
    Latency      813.28us    71.94us     2.75ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.71MB/s
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
    Reqs/sec     19447.83    9029.90   80607.65
    Latency        2.56ms     2.46ms   215.17ms
    HTTP codes:
      1xx - 0, 2xx - 91103, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8897
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8897
    Throughput:     4.01MB/s
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
    Reqs/sec     25374.10    8817.04   77391.57
    Latency        1.97ms     2.25ms   187.46ms
    HTTP codes:
      1xx - 0, 2xx - 96638, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3362
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3362
    Throughput:     5.61MB/s
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
    Reqs/sec     73807.79    2722.08   81533.78
    Latency      674.66us   131.47us     6.67ms
    HTTP codes:
      1xx - 0, 2xx - 97375, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2625
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2625
    Throughput:    11.37MB/s
  ```


