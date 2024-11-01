## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74536` | `6109` | `81675` |
| **83%** | [Hyper Express](#hyper-express) | `62164` | `3722` | `64917` |
| **35%** | [Node (Default)](#node-default) | `26055` | `6794` | `48978` |
| **33%** | [Fastify](#fastify) | `24726` | `7381` | `36395` |
| **29%** | [Hono](#hono) | `21375` | `5546` | `29843` |
| **24%** | [Koa](#koa) | `17828` | `6074` | `45454` |
| **11%** | [Carbon](#carbon) | `8415` | `1452` | `10708` |
| **9%** | [Express](#express) | `6726` | `1078` | `8704` |


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
    Reqs/sec      8829.90    3882.62   51636.96
    Latency        5.65ms     4.11ms   354.76ms
    HTTP codes:
      1xx - 0, 2xx - 94771, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5229
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5229
    Throughput:     1.90MB/s
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
    Reqs/sec      6615.31    1041.14    8695.16
    Latency        7.55ms     3.48ms   332.34ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.89MB/s
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
    Reqs/sec     24878.95    7350.89   36111.17
    Latency        2.01ms     2.01ms   180.85ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.64MB/s
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
    Reqs/sec     21087.77    5720.55   30085.80
    Latency        2.37ms     2.03ms   182.71ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.76MB/s
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
    Reqs/sec     62687.38    3450.96   66377.27
    Latency      795.46us    65.86us     4.22ms
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
    Reqs/sec     18583.48    6056.78   45136.93
    Latency        2.68ms     2.08ms   187.28ms
    HTTP codes:
      1xx - 0, 2xx - 95212, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4788
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4788
    Throughput:     4.00MB/s
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
    Reqs/sec     27234.63    8485.93   49389.34
    Latency        1.82ms     1.96ms   170.17ms
    HTTP codes:
      1xx - 0, 2xx - 97274, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2726
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2726
    Throughput:     6.09MB/s
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
    Reqs/sec     75081.68    5699.59   83981.64
    Latency      663.65us   189.30us     7.87ms
    HTTP codes:
      1xx - 0, 2xx - 97255, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2745
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2745
    Throughput:    11.56MB/s
  ```


