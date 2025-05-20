## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73033` | `5077` | `83139` |
| **85%** | [Hyper Express](#hyper-express) | `61901` | `3313` | `66118` |
| **35%** | [Node (Default)](#node-default) | `25510` | `7884` | `49488` |
| **33%** | [Fastify](#fastify) | `24298` | `7588` | `37084` |
| **28%** | [Hono](#hono) | `20541` | `5740` | `30146` |
| **26%** | [Koa](#koa) | `19073` | `7168` | `54883` |
| **11%** | [Carbon](#carbon) | `8015` | `1329` | `10213` |
| **9%** | [Express](#express) | `6444` | `1056` | `8440` |


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
    Reqs/sec      8626.27    4899.93   55734.73
    Latency        5.78ms     4.33ms   372.31ms
    HTTP codes:
      1xx - 0, 2xx - 92241, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7759
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7759
    Throughput:     1.81MB/s
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
    Reqs/sec      6333.05     995.01    8425.66
    Latency        7.89ms     3.71ms   354.58ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     24389.78    7308.55   36248.59
    Latency        2.05ms     1.97ms   180.53ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.53MB/s
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
    Reqs/sec     21173.44    6140.29   30071.80
    Latency        2.36ms     2.12ms   188.85ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.78MB/s
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
    Reqs/sec     61463.27    3435.81   65160.32
    Latency      811.58us    70.60us     3.29ms
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
    Reqs/sec     18835.21    7287.16   56453.66
    Latency        2.65ms     2.35ms   208.35ms
    HTTP codes:
      1xx - 0, 2xx - 92955, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7045
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7045
    Throughput:     3.96MB/s
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
    Reqs/sec     25992.58    7905.86   56949.42
    Latency        1.92ms     1.96ms   170.51ms
    HTTP codes:
      1xx - 0, 2xx - 97602, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2398
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2398
    Throughput:     5.80MB/s
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
    Reqs/sec     73039.41    4685.20   83700.04
    Latency      682.18us   147.50us     5.83ms
    HTTP codes:
      1xx - 0, 2xx - 97677, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2323
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2323
    Throughput:    11.29MB/s
  ```


