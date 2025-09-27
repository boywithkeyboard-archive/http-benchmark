## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75589` | `3597` | `83962` |
| **82%** | [Hyper Express](#hyper-express) | `62307` | `3719` | `66198` |
| **32%** | [Node (Default)](#node-default) | `23988` | `6430` | `66312` |
| **29%** | [Fastify](#fastify) | `21672` | `5122` | `36552` |
| **25%** | [Hono](#hono) | `18765` | `4639` | `30305` |
| **21%** | [Koa](#koa) | `16228` | `5161` | `52490` |
| **11%** | [Carbon](#carbon) | `8011` | `1376` | `10351` |
| **9%** | [Express](#express) | `6478` | `1128` | `8339` |


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
    Reqs/sec      8977.56    6167.77   81375.09
    Latency        5.56ms     4.31ms   378.20ms
    HTTP codes:
      1xx - 0, 2xx - 90853, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9147
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9147
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
    Reqs/sec      6271.05     985.53    8304.28
    Latency        7.97ms     3.58ms   343.74ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.79MB/s
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
    Reqs/sec     21783.57    5233.04   36659.97
    Latency        2.29ms     2.00ms   179.76ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.94MB/s
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
    Reqs/sec     18394.70    4558.48   29911.38
    Latency        2.72ms     2.09ms   184.97ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.16MB/s
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
    Reqs/sec     62870.39    3731.59   66822.86
    Latency      792.85us    68.92us     2.82ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.93MB/s
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
    Reqs/sec     17759.72   10139.72   82835.00
    Latency        2.80ms     2.38ms   207.94ms
    HTTP codes:
      1xx - 0, 2xx - 87810, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12190
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12190
    Throughput:     3.53MB/s
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
    Reqs/sec     22937.07    6845.34   75617.55
    Latency        2.18ms     1.98ms   168.44ms
    HTTP codes:
      1xx - 0, 2xx - 96139, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3861
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3861
    Throughput:     5.04MB/s
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
    Reqs/sec     76207.07    3084.09   84179.23
    Latency      653.56us   119.67us     5.11ms
    HTTP codes:
      1xx - 0, 2xx - 97525, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2475
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2475
    Throughput:    11.76MB/s
  ```


