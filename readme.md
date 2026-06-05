## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73494` | `3828` | `84846` |
| **82%** | [Hyper Express](#hyper-express) | `60631` | `3981` | `70217` |
| **30%** | [Hono](#hono) | `22042` | `6778` | `30944` |
| **29%** | [Fastify](#fastify) | `21389` | `6273` | `36381` |
| **29%** | [Node (Default)](#node-default) | `21003` | `5503` | `65878` |
| **26%** | [Koa](#koa) | `19197` | `8678` | `70518` |
| **10%** | [Carbon](#carbon) | `7411` | `1200` | `10497` |
| **9%** | [Express](#express) | `6314` | `1110` | `8387` |


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
    Reqs/sec      8122.29    6127.90   83658.37
    Latency        6.15ms     4.28ms   364.99ms
    HTTP codes:
      1xx - 0, 2xx - 90722, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9278
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9278
    Throughput:     1.67MB/s
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
    Reqs/sec      6292.30    1082.83    8479.26
    Latency        7.94ms     3.81ms   361.67ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     21018.99    5744.35   37743.14
    Latency        2.38ms     2.14ms   196.25ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.77MB/s
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
    Reqs/sec     22282.87    6772.95   32066.03
    Latency        2.24ms     1.99ms   181.68ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.04MB/s
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
    Reqs/sec     60533.35    3788.54   66196.80
    Latency      823.82us    98.10us     3.70ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.60MB/s
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
    Reqs/sec     19501.72    9274.85   80161.31
    Latency        2.56ms     2.45ms   209.94ms
    HTTP codes:
      1xx - 0, 2xx - 91987, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8013
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8013
    Throughput:     4.05MB/s
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
    Reqs/sec     20292.87    5435.24   70157.34
    Latency        2.45ms     2.02ms   177.66ms
    HTTP codes:
      1xx - 0, 2xx - 96160, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3840
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3840
    Throughput:     4.48MB/s
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
    Reqs/sec     71997.33    2845.66   79444.96
    Latency      691.90us   190.10us    10.12ms
    HTTP codes:
      1xx - 0, 2xx - 93970, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6030
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6030
    Throughput:    10.70MB/s
  ```


