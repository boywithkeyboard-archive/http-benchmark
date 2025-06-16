## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73840` | `5515` | `81662` |
| **85%** | [Hyper Express](#hyper-express) | `62727` | `4115` | `82236` |
| **35%** | [Node (Default)](#node-default) | `25937` | `7953` | `56087` |
| **31%** | [Fastify](#fastify) | `23012` | `6608` | `35545` |
| **28%** | [Hono](#hono) | `20446` | `5884` | `29859` |
| **26%** | [Koa](#koa) | `19091` | `7115` | `59031` |
| **11%** | [Carbon](#carbon) | `8280` | `1461` | `10407` |
| **9%** | [Express](#express) | `6512` | `1079` | `8465` |


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
    Reqs/sec      8843.08    4808.04   51222.08
    Latency        5.64ms     4.32ms   370.04ms
    HTTP codes:
      1xx - 0, 2xx - 92008, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7992
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7992
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
    Reqs/sec      6942.39    4955.68   62393.96
    Latency        7.19ms     3.98ms   364.15ms
    HTTP codes:
      1xx - 0, 2xx - 90568, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9432
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9432
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
    Reqs/sec     24292.65    8039.46   36613.42
    Latency        2.06ms     2.10ms   184.38ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.50MB/s
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
    Reqs/sec     20766.06    5954.90   30942.88
    Latency        2.41ms     2.13ms   192.40ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.69MB/s
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
    Reqs/sec     62296.64    3619.50   65419.78
    Latency      799.96us    77.00us     3.18ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.85MB/s
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
    Reqs/sec     18170.14    6165.09   48247.88
    Latency        2.75ms     2.35ms   205.76ms
    HTTP codes:
      1xx - 0, 2xx - 94791, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5209
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5209
    Throughput:     3.89MB/s
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
    Reqs/sec     25144.32    7603.95   49083.25
    Latency        1.99ms     1.91ms   164.92ms
    HTTP codes:
      1xx - 0, 2xx - 97961, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2039
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2039
    Throughput:     5.64MB/s
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
    Reqs/sec     73670.16    5123.41   82139.24
    Latency      676.43us   169.31us     6.11ms
    HTTP codes:
      1xx - 0, 2xx - 97332, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2668
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2668
    Throughput:    11.35MB/s
  ```


