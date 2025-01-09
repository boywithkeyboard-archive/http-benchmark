## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73128` | `5637` | `76742` |
| **85%** | [Hyper Express](#hyper-express) | `62247` | `3873` | `81707` |
| **35%** | [Node (Default)](#node-default) | `25816` | `7455` | `59032` |
| **33%** | [Fastify](#fastify) | `23990` | `7068` | `34924` |
| **28%** | [Hono](#hono) | `20136` | `5141` | `28885` |
| **24%** | [Koa](#koa) | `17860` | `6189` | `50783` |
| **11%** | [Carbon](#carbon) | `8381` | `1451` | `10409` |
| **9%** | [Express](#express) | `6386` | `981` | `8439` |


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
    Reqs/sec      8500.66    3522.17   55329.12
    Latency        5.87ms     4.24ms   368.81ms
    HTTP codes:
      1xx - 0, 2xx - 95328, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4672
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4672
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
    Reqs/sec      6415.32     985.80    8570.11
    Latency        7.79ms     3.56ms   337.91ms
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
    Reqs/sec     23932.38    7039.09   35089.13
    Latency        2.09ms     2.02ms   183.59ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.42MB/s
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
    Reqs/sec     20244.46    5330.28   28526.73
    Latency        2.47ms     1.89ms   172.82ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.57MB/s
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
    Reqs/sec     62512.65    3631.84   67523.93
    Latency      797.64us    76.52us     3.66ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.88MB/s
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
    Reqs/sec     18591.81    6810.73   55388.16
    Latency        2.68ms     2.37ms   205.07ms
    HTTP codes:
      1xx - 0, 2xx - 93786, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6214
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6214
    Throughput:     3.94MB/s
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
    Reqs/sec     24994.82    6872.73   40011.15
    Latency        1.99ms     1.99ms   167.63ms
    HTTP codes:
      1xx - 0, 2xx - 98312, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1688
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1688
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
    Reqs/sec     73269.64    5785.92   85746.05
    Latency      676.23us   151.97us    11.17ms
    HTTP codes:
      1xx - 0, 2xx - 97884, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2116
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2116
    Throughput:    11.35MB/s
  ```


