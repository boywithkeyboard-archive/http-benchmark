## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74897` | `3038` | `82642` |
| **83%** | [Hyper Express](#hyper-express) | `62158` | `4007` | `67494` |
| **34%** | [Node (Default)](#node-default) | `25526` | `7655` | `61415` |
| **34%** | [Fastify](#fastify) | `25402` | `8184` | `36118` |
| **28%** | [Hono](#hono) | `20800` | `5753` | `28738` |
| **26%** | [Koa](#koa) | `19481` | `9328` | `78525` |
| **11%** | [Carbon](#carbon) | `8144` | `1385` | `10334` |
| **8%** | [Express](#express) | `6346` | `1039` | `8378` |


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
    Reqs/sec      8885.24    6538.28   73835.43
    Latency        5.62ms     4.44ms   381.12ms
    HTTP codes:
      1xx - 0, 2xx - 90682, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9318
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9318
    Throughput:     1.83MB/s
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
    Reqs/sec      7089.52    6417.02   74748.83
    Latency        7.04ms     3.99ms   363.38ms
    HTTP codes:
      1xx - 0, 2xx - 88837, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11163
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11163
    Throughput:     1.80MB/s
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
    Reqs/sec     25037.03    8197.09   36955.62
    Latency        1.99ms     1.97ms   175.67ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.68MB/s
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
    Reqs/sec     20444.83    5562.35   29543.51
    Latency        2.44ms     2.08ms   188.88ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.62MB/s
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
    Reqs/sec     63407.91    6813.08   79457.15
    Latency      786.95us    85.76us     3.88ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.00MB/s
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
    Reqs/sec     19025.36    6680.85   57755.81
    Latency        2.62ms     2.54ms   221.04ms
    HTTP codes:
      1xx - 0, 2xx - 94710, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5290
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5290
    Throughput:     4.08MB/s
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
    Reqs/sec     25268.87    8111.92   65221.30
    Latency        1.98ms     1.94ms   165.18ms
    HTTP codes:
      1xx - 0, 2xx - 97316, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2684
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2684
    Throughput:     5.62MB/s
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
    Reqs/sec     75072.41    2490.52   80595.35
    Latency      663.67us   137.33us     6.25ms
    HTTP codes:
      1xx - 0, 2xx - 96870, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3130
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3130
    Throughput:    11.51MB/s
  ```


