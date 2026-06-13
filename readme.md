## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `79464` | `3081` | `86048` |
| **88%** | [Hyper Express](#hyper-express) | `69910` | `3284` | `73634` |
| **47%** | [Node (Default)](#node-default) | `37597` | `11289` | `82531` |
| **43%** | [Fastify](#fastify) | `34018` | `9120` | `52082` |
| **37%** | [Koa](#koa) | `29429` | `14478` | `89260` |
| **35%** | [Hono](#hono) | `28020` | `7237` | `46433` |
| **12%** | [Carbon](#carbon) | `9391` | `2327` | `13882` |
| **9%** | [Express](#express) | `7534` | `1743` | `10680` |


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
    Reqs/sec     10636.66    8116.52   93774.11
    Latency        4.70ms     4.28ms   367.71ms
    HTTP codes:
      1xx - 0, 2xx - 89639, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10361
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10361
    Throughput:     2.16MB/s
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
    Reqs/sec      8199.16    6193.56   75258.31
    Latency        6.09ms     3.92ms   355.38ms
    HTTP codes:
      1xx - 0, 2xx - 91155, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8845
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8845
    Throughput:     2.14MB/s
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
    Reqs/sec     33655.54    9877.70   52375.26
    Latency        1.48ms     1.91ms   166.52ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.63MB/s
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
    Reqs/sec     28527.58    6967.03   47486.40
    Latency        1.75ms     1.90ms   168.76ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     6.44MB/s
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
    Reqs/sec     69944.09    2545.22   77318.14
    Latency      712.84us   139.95us     8.25ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.94MB/s
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
    Reqs/sec     28708.98   11705.95   77101.86
    Latency        1.74ms     2.44ms   209.67ms
    HTTP codes:
      1xx - 0, 2xx - 91865, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8135
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8135
    Throughput:     5.97MB/s
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
    Reqs/sec     34665.02    9963.00   75565.11
    Latency        1.44ms     1.78ms   150.98ms
    HTTP codes:
      1xx - 0, 2xx - 96447, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3553
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3553
    Throughput:     7.65MB/s
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
    Reqs/sec     79275.96    2936.60   85197.38
    Latency      627.95us   190.23us    12.78ms
    HTTP codes:
      1xx - 0, 2xx - 96232, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3768
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3768
    Throughput:    12.07MB/s
  ```


