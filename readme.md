## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73628` | `5241` | `81761` |
| **82%** | [Hyper Express](#hyper-express) | `60209` | `5625` | `66689` |
| **36%** | [Node (Default)](#node-default) | `26544` | `7816` | `40567` |
| **33%** | [Fastify](#fastify) | `24277` | `7747` | `36573` |
| **29%** | [Hono](#hono) | `21106` | `6023` | `30322` |
| **26%** | [Koa](#koa) | `19055` | `6905` | `60171` |
| **11%** | [Carbon](#carbon) | `8398` | `1438` | `10626` |
| **9%** | [Express](#express) | `6586` | `1049` | `8495` |


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
    Reqs/sec      8904.20    4737.16   65063.83
    Latency        5.61ms     4.14ms   357.86ms
    HTTP codes:
      1xx - 0, 2xx - 92776, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7224
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7224
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
    Reqs/sec      6824.91    4059.84   54720.88
    Latency        7.32ms     3.88ms   351.81ms
    HTTP codes:
      1xx - 0, 2xx - 92679, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7321
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7321
    Throughput:     1.81MB/s
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
    Reqs/sec     23619.89    6931.46   35496.26
    Latency        2.12ms     1.99ms   178.63ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.36MB/s
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
    Reqs/sec     21047.28    6012.52   30357.65
    Latency        2.37ms     1.95ms   174.19ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.76MB/s
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
    Reqs/sec     63246.40    3070.40   65865.94
    Latency      788.21us    66.38us     4.36ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.98MB/s
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
    Reqs/sec     18203.03    7038.68   65601.74
    Latency        2.74ms     2.27ms   198.42ms
    HTTP codes:
      1xx - 0, 2xx - 92860, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7140
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7140
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
    Reqs/sec     25782.91    7506.69   47758.93
    Latency        1.94ms     1.81ms   154.83ms
    HTTP codes:
      1xx - 0, 2xx - 97660, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2340
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2340
    Throughput:     5.76MB/s
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
    Reqs/sec     74655.46    6951.35   93917.54
    Latency      662.64us   132.76us     9.72ms
    HTTP codes:
      1xx - 0, 2xx - 97784, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2216
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2214
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 2
    Throughput:    11.55MB/s
  ```


