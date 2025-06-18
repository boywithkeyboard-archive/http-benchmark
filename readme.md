## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73160` | `5128` | `81470` |
| **84%** | [Hyper Express](#hyper-express) | `61547` | `3286` | `64606` |
| **35%** | [Node (Default)](#node-default) | `25921` | `6988` | `48132` |
| **33%** | [Fastify](#fastify) | `24276` | `7278` | `35606` |
| **29%** | [Hono](#hono) | `21115` | `6102` | `31452` |
| **24%** | [Koa](#koa) | `17750` | `6552` | `54111` |
| **11%** | [Carbon](#carbon) | `8180` | `1474` | `10392` |
| **9%** | [Express](#express) | `6423` | `1105` | `8461` |


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
    Reqs/sec      8841.80    5156.97   55256.81
    Latency        5.65ms     4.32ms   375.34ms
    HTTP codes:
      1xx - 0, 2xx - 91417, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8583
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8583
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
    Reqs/sec      6429.78    1074.33    8415.70
    Latency        7.77ms     3.76ms   362.50ms
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
    Reqs/sec     25076.42    8049.47   37673.57
    Latency        1.99ms     1.95ms   179.19ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.69MB/s
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
    Reqs/sec     21352.05    6035.92   31312.25
    Latency        2.34ms     1.99ms   179.84ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.83MB/s
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
    Reqs/sec     62020.39    3747.19   69040.65
    Latency      804.16us    75.83us     3.16ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.81MB/s
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
    Reqs/sec     18323.20    6791.27   52697.03
    Latency        2.72ms     2.47ms   214.72ms
    HTTP codes:
      1xx - 0, 2xx - 94123, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5877
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5877
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
    Reqs/sec     25258.40    7435.40   59174.92
    Latency        1.98ms     1.89ms   161.23ms
    HTTP codes:
      1xx - 0, 2xx - 96968, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3032
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3032
    Throughput:     5.60MB/s
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
    Reqs/sec     73791.52    4610.79   81946.58
    Latency      674.02us   217.82us    10.83ms
    HTTP codes:
      1xx - 0, 2xx - 96417, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3583
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3583
    Throughput:    11.26MB/s
  ```


