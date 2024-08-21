## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73017` | `5726` | `76124` |
| **86%** | [Hyper Express](#hyper-express) | `62997` | `2860` | `68656` |
| **35%** | [Node (Default)](#node-default) | `25414` | `7193` | `49597` |
| **33%** | [Fastify](#fastify) | `24191` | `7204` | `34293` |
| **28%** | [Hono](#hono) | `20759` | `5920` | `29387` |
| **24%** | [Koa](#koa) | `17558` | `6205` | `56574` |
| **11%** | [Carbon](#carbon) | `8379` | `1425` | `10552` |
| **9%** | [Express](#express) | `6526` | `1038` | `8558` |


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
    Reqs/sec      8576.95    3711.00   45216.95
    Latency        5.82ms     4.15ms   357.79ms
    HTTP codes:
      1xx - 0, 2xx - 94930, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5070
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5070
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
    Reqs/sec      6545.33    1093.28    8472.39
    Latency        7.64ms     3.60ms   347.40ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.87MB/s
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
    Reqs/sec     24232.51    7141.90   35051.25
    Latency        2.06ms     2.05ms   181.01ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.49MB/s
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
    Reqs/sec     19307.35    5096.27   28407.79
    Latency        2.59ms     1.93ms   177.54ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.36MB/s
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
    Reqs/sec     63048.46    4156.55   74298.17
    Latency      789.68us    67.52us     3.09ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.97MB/s
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
    Reqs/sec     17710.78    6047.69   58467.15
    Latency        2.81ms     2.28ms   206.79ms
    HTTP codes:
      1xx - 0, 2xx - 94725, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5275
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5275
    Throughput:     3.80MB/s
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
    Reqs/sec     25836.94    7714.02   59611.58
    Latency        1.93ms     1.93ms   164.64ms
    HTTP codes:
      1xx - 0, 2xx - 97223, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2777
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2761
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 16
    Throughput:     5.75MB/s
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
    Reqs/sec     73576.14    5468.57   77385.25
    Latency      677.24us   226.33us    17.97ms
    HTTP codes:
      1xx - 0, 2xx - 97598, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2402
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2391
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 11
    Throughput:    11.36MB/s
  ```


