## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74335` | `3645` | `82241` |
| **83%** | [Hyper Express](#hyper-express) | `61822` | `4205` | `76096` |
| **35%** | [Node (Default)](#node-default) | `25655` | `8351` | `71746` |
| **31%** | [Fastify](#fastify) | `23175` | `6726` | `35706` |
| **27%** | [Hono](#hono) | `20439` | `5757` | `29300` |
| **24%** | [Koa](#koa) | `17741` | `7993` | `70245` |
| **11%** | [Carbon](#carbon) | `8161` | `1425` | `10422` |
| **8%** | [Express](#express) | `6228` | `1003` | `8602` |


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
    Reqs/sec      8958.14    5498.00   67941.50
    Latency        5.57ms     4.37ms   372.82ms
    HTTP codes:
      1xx - 0, 2xx - 92695, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7305
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7305
    Throughput:     1.89MB/s
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
    Reqs/sec      6400.52    1059.37   10575.91
    Latency        7.81ms     3.76ms   359.53ms
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
    Reqs/sec     23386.39    7126.29   35765.67
    Latency        2.14ms     2.03ms   186.06ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.30MB/s
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
    Reqs/sec     20663.89    6244.63   30135.73
    Latency        2.42ms     2.01ms   181.65ms
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
    Reqs/sec     62597.61    3755.64   65464.54
    Latency      796.60us    71.24us     3.68ms
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
    Reqs/sec     17488.88    7720.22   66883.76
    Latency        2.85ms     2.45ms   214.81ms
    HTTP codes:
      1xx - 0, 2xx - 93155, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6845
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6845
    Throughput:     3.68MB/s
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
    Reqs/sec     24978.18    8011.28   79076.66
    Latency        2.00ms     1.98ms   168.56ms
    HTTP codes:
      1xx - 0, 2xx - 96507, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3493
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3493
    Throughput:     5.51MB/s
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
    Reqs/sec     75020.77    2605.87   83209.09
    Latency      663.56us   170.27us    10.16ms
    HTTP codes:
      1xx - 0, 2xx - 96454, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3546
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3546
    Throughput:    11.45MB/s
  ```


