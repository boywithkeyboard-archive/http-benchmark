## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `80146` | `2207` | `85965` |
| **85%** | [Hyper Express](#hyper-express) | `68427` | `5249` | `74265` |
| **42%** | [Node (Default)](#node-default) | `33273` | `8328` | `65425` |
| **39%** | [Fastify](#fastify) | `31315` | `7820` | `50610` |
| **36%** | [Hono](#hono) | `28827` | `8846` | `47953` |
| **35%** | [Koa](#koa) | `28300` | `11304` | `65888` |
| **12%** | [Carbon](#carbon) | `9559` | `2476` | `13412` |
| **9%** | [Express](#express) | `7546` | `1754` | `10563` |


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
    Reqs/sec     10422.78    7333.24   74887.87
    Latency        4.79ms     4.29ms   369.12ms
    HTTP codes:
      1xx - 0, 2xx - 90652, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9348
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9348
    Throughput:     2.15MB/s
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
    Reqs/sec      7984.73    6744.52   76783.60
    Latency        6.25ms     3.78ms   351.60ms
    HTTP codes:
      1xx - 0, 2xx - 89570, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10430
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10427
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 3
    Throughput:     2.05MB/s
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
    Reqs/sec     36705.52   12250.38   52174.42
    Latency        1.36ms     2.03ms   174.34ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.33MB/s
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
    Reqs/sec     28568.91    7763.93   47052.16
    Latency        1.75ms     2.06ms   176.17ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     6.45MB/s
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
    Reqs/sec     69050.12    3284.54   73139.85
    Latency      722.26us    73.48us     3.02ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.81MB/s
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
    Reqs/sec     29925.27   13650.77   83568.37
    Latency        1.67ms     2.33ms   202.74ms
    HTTP codes:
      1xx - 0, 2xx - 90474, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9526
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9526
    Throughput:     6.11MB/s
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
    Reqs/sec     35827.00   10926.96   74753.05
    Latency        1.39ms     1.79ms   144.37ms
    HTTP codes:
      1xx - 0, 2xx - 96705, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3295
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3295
    Throughput:     7.94MB/s
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
    Reqs/sec     80815.29    3014.29   86269.47
    Latency      616.08us   166.28us     8.95ms
    HTTP codes:
      1xx - 0, 2xx - 94554, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5446
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5446
    Throughput:    12.10MB/s
  ```


