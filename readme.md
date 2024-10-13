## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74671` | `5059` | `78616` |
| **86%** | [Hyper Express](#hyper-express) | `63893` | `3080` | `69329` |
| **35%** | [Node (Default)](#node-default) | `26176` | `7780` | `40022` |
| **33%** | [Fastify](#fastify) | `24518` | `7276` | `36895` |
| **29%** | [Hono](#hono) | `21490` | `6048` | `30865` |
| **25%** | [Koa](#koa) | `18660` | `6800` | `59534` |
| **11%** | [Carbon](#carbon) | `8284` | `1454` | `10537` |
| **9%** | [Express](#express) | `6450` | `1029` | `8451` |


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
    Reqs/sec      8567.79    2883.52   52484.27
    Latency        5.80ms     4.17ms   356.35ms
    HTTP codes:
      1xx - 0, 2xx - 96403, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3597
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3597
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
    Reqs/sec      6451.68    1030.78    8497.51
    Latency        7.75ms     3.71ms   352.90ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.85MB/s
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
    Reqs/sec     24858.09    7567.11   37754.76
    Latency        2.01ms     1.96ms   175.25ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.63MB/s
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
    Reqs/sec     20950.54    5847.20   31386.61
    Latency        2.38ms     1.94ms   177.90ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.73MB/s
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
    Reqs/sec     64362.84    3519.34   74036.52
    Latency      774.82us    66.09us     3.31ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.14MB/s
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
    Reqs/sec     18856.24    6691.02   60404.99
    Latency        2.65ms     2.28ms   200.34ms
    HTTP codes:
      1xx - 0, 2xx - 94210, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5790
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5790
    Throughput:     4.02MB/s
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
    Reqs/sec     25041.15    7090.65   41879.98
    Latency        1.99ms     1.83ms   161.21ms
    HTTP codes:
      1xx - 0, 2xx - 98252, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1748
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1748
    Throughput:     5.63MB/s
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
    Reqs/sec     77296.24    5780.75   81437.98
    Latency      643.55us   187.49us    12.37ms
    HTTP codes:
      1xx - 0, 2xx - 98054, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1946
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1946
    Throughput:    11.98MB/s
  ```


