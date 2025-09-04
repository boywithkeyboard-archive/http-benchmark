## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75303` | `2839` | `84278` |
| **84%** | [Hyper Express](#hyper-express) | `62885` | `3774` | `66606` |
| **35%** | [Node (Default)](#node-default) | `26114` | `8296` | `59972` |
| **32%** | [Fastify](#fastify) | `24330` | `7290` | `35841` |
| **28%** | [Hono](#hono) | `20751` | `5809` | `30329` |
| **26%** | [Koa](#koa) | `19646` | `9302` | `77453` |
| **11%** | [Carbon](#carbon) | `8217` | `1435` | `10399` |
| **9%** | [Express](#express) | `6513` | `1104` | `8429` |


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
    Reqs/sec      9142.31    6298.29   69763.64
    Latency        5.46ms     4.40ms   378.27ms
    HTTP codes:
      1xx - 0, 2xx - 90744, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9256
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9256
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
    Reqs/sec      7200.20    6604.22   77739.97
    Latency        6.93ms     4.03ms   368.37ms
    HTTP codes:
      1xx - 0, 2xx - 88560, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11440
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11440
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
    Reqs/sec     22887.69    6637.90   35296.70
    Latency        2.18ms     2.10ms   185.98ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.19MB/s
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
    Reqs/sec     20152.08    5289.25   29067.82
    Latency        2.48ms     2.05ms   183.05ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.55MB/s
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
    Reqs/sec     63296.34    3723.95   66465.65
    Latency      787.82us    73.77us     4.27ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.99MB/s
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
    Reqs/sec     19599.24    9326.89   79027.32
    Latency        2.55ms     2.25ms   199.03ms
    HTTP codes:
      1xx - 0, 2xx - 90755, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9245
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9245
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
    Reqs/sec     25430.70    7770.29   66323.11
    Latency        1.96ms     1.92ms   164.56ms
    HTTP codes:
      1xx - 0, 2xx - 97244, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2756
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2756
    Throughput:     5.66MB/s
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
    Reqs/sec     74006.96    3004.67   79611.76
    Latency      673.14us   140.91us     5.32ms
    HTTP codes:
      1xx - 0, 2xx - 97375, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2625
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2625
    Throughput:    11.41MB/s
  ```


