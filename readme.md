## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73273` | `5336` | `79985` |
| **85%** | [Hyper Express](#hyper-express) | `61963` | `3737` | `65582` |
| **33%** | [Node (Default)](#node-default) | `24066` | `6559` | `52260` |
| **29%** | [Fastify](#fastify) | `21170` | `4717` | `36286` |
| **27%** | [Hono](#hono) | `19646` | `5153` | `30234` |
| **24%** | [Koa](#koa) | `17825` | `6031` | `52188` |
| **11%** | [Carbon](#carbon) | `8343` | `1434` | `10318` |
| **9%** | [Express](#express) | `6538` | `1121` | `8541` |


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
    Reqs/sec      8759.98    4683.30   57222.49
    Latency        5.70ms     4.25ms   368.24ms
    HTTP codes:
      1xx - 0, 2xx - 92855, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7145
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7145
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
    Reqs/sec      6431.72    1076.35    8487.92
    Latency        7.77ms     3.68ms   352.87ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.84MB/s
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
    Reqs/sec     22682.79    6449.16   36250.64
    Latency        2.20ms     2.00ms   178.12ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.15MB/s
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
    Reqs/sec     19016.18    4543.62   29288.29
    Latency        2.63ms     2.06ms   184.67ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.30MB/s
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
    Reqs/sec     61913.05    3437.10   64397.09
    Latency      805.51us    77.36us     5.11ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.80MB/s
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
    Reqs/sec     17061.12    5938.35   52790.99
    Latency        2.92ms     2.42ms   212.57ms
    HTTP codes:
      1xx - 0, 2xx - 94700, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5300
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5300
    Throughput:     3.65MB/s
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
    Reqs/sec     24343.17    7058.71   59945.91
    Latency        2.05ms     2.04ms   170.19ms
    HTTP codes:
      1xx - 0, 2xx - 96719, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3281
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3281
    Throughput:     5.40MB/s
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
    Reqs/sec     73832.46    4834.87   82526.12
    Latency      674.59us   168.83us     6.26ms
    HTTP codes:
      1xx - 0, 2xx - 97735, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2265
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2265
    Throughput:    11.42MB/s
  ```


