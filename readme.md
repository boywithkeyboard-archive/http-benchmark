## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75477` | `5568` | `82887` |
| **84%** | [Hyper Express](#hyper-express) | `63431` | `5074` | `82882` |
| **35%** | [Node (Default)](#node-default) | `26741` | `7336` | `50713` |
| **32%** | [Fastify](#fastify) | `24147` | `7273` | `36176` |
| **29%** | [Hono](#hono) | `22036` | `6083` | `30783` |
| **25%** | [Koa](#koa) | `18678` | `6860` | `55490` |
| **11%** | [Carbon](#carbon) | `8602` | `1509` | `10606` |
| **9%** | [Express](#express) | `6461` | `1012` | `8406` |


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
    Reqs/sec      8975.12    4678.54   60797.15
    Latency        5.56ms     4.24ms   366.61ms
    HTTP codes:
      1xx - 0, 2xx - 93779, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6221
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6221
    Throughput:     1.91MB/s
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
    Reqs/sec      6949.25    4210.95   54793.66
    Latency        7.19ms     3.85ms   351.16ms
    HTTP codes:
      1xx - 0, 2xx - 92632, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7368
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7368
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
    Reqs/sec     24490.40    7412.05   36369.39
    Latency        2.04ms     1.87ms   167.60ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.56MB/s
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
    Reqs/sec     21007.83    5863.31   29231.15
    Latency        2.38ms     2.01ms   179.52ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.74MB/s
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
    Reqs/sec     64166.43    4232.69   71670.42
    Latency      776.84us    78.36us     3.33ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.12MB/s
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
    Reqs/sec     18817.35    6465.07   51208.27
    Latency        2.65ms     2.34ms   202.36ms
    HTTP codes:
      1xx - 0, 2xx - 94737, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5263
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5263
    Throughput:     4.03MB/s
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
    Reqs/sec     28539.58    8676.07   58248.12
    Latency        1.75ms     1.79ms   154.88ms
    HTTP codes:
      1xx - 0, 2xx - 97174, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2826
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2826
    Throughput:     6.34MB/s
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
    Reqs/sec     74576.76    5363.43   90502.76
    Latency      667.57us   157.16us     7.40ms
    HTTP codes:
      1xx - 0, 2xx - 96831, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3169
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3169
    Throughput:    11.43MB/s
  ```


