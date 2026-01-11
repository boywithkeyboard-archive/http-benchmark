## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73682` | `1873` | `76096` |
| **84%** | [Hyper Express](#hyper-express) | `61592` | `3544` | `66126` |
| **34%** | [Node (Default)](#node-default) | `25072` | `9122` | `78117` |
| **32%** | [Fastify](#fastify) | `23370` | `7316` | `34729` |
| **27%** | [Hono](#hono) | `20055` | `5929` | `29334` |
| **25%** | [Koa](#koa) | `18691` | `9830` | `79573` |
| **11%** | [Carbon](#carbon) | `8088` | `1418` | `10208` |
| **8%** | [Express](#express) | `6177` | `958` | `8184` |


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
    Reqs/sec      8947.21    7282.08   83438.73
    Latency        5.58ms     4.35ms   373.31ms
    HTTP codes:
      1xx - 0, 2xx - 88771, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11229
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11229
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
    Reqs/sec      6739.64    5339.96   66243.03
    Latency        7.40ms     4.07ms   377.53ms
    HTTP codes:
      1xx - 0, 2xx - 90920, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9080
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9080
    Throughput:     1.76MB/s
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
    Reqs/sec     22894.94    6931.60   35155.00
    Latency        2.18ms     2.09ms   186.28ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.20MB/s
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
    Reqs/sec     20753.95    6418.51   29729.41
    Latency        2.41ms     2.05ms   182.31ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.68MB/s
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
    Reqs/sec     61462.19    3761.07   66846.30
    Latency      811.45us    71.83us     2.86ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.73MB/s
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
    Reqs/sec     18681.96    9419.13   78040.03
    Latency        2.67ms     2.55ms   222.76ms
    HTTP codes:
      1xx - 0, 2xx - 90755, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9245
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9245
    Throughput:     3.83MB/s
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
    Reqs/sec     24953.11    8283.91   71774.55
    Latency        2.00ms     2.08ms   178.85ms
    HTTP codes:
      1xx - 0, 2xx - 96447, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3553
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3553
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
    Reqs/sec     74201.58    2805.15   79801.88
    Latency      670.49us   202.94us    11.05ms
    HTTP codes:
      1xx - 0, 2xx - 96504, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3496
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3496
    Throughput:    11.33MB/s
  ```


