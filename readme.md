## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `76966` | `6605` | `80669` |
| **82%** | [Hyper Express](#hyper-express) | `63096` | `3931` | `77425` |
| **36%** | [Node (Default)](#node-default) | `27605` | `9091` | `61355` |
| **29%** | [Fastify](#fastify) | `22652` | `6656` | `36045` |
| **26%** | [Hono](#hono) | `19816` | `5565` | `29556` |
| **24%** | [Koa](#koa) | `18390` | `6498` | `60743` |
| **11%** | [Carbon](#carbon) | `8292` | `1425` | `10496` |
| **8%** | [Express](#express) | `6515` | `1005` | `8515` |


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
    Reqs/sec      8618.19    4793.35   54035.16
    Latency        5.79ms     4.17ms   360.59ms
    HTTP codes:
      1xx - 0, 2xx - 92772, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7228
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7228
    Throughput:     1.82MB/s
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
    Reqs/sec      6469.48    1044.42    8484.75
    Latency        7.73ms     3.62ms   344.75ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.85MB/s
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
    Reqs/sec     24016.78    7453.89   36098.41
    Latency        2.08ms     1.95ms   176.21ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.44MB/s
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
    Reqs/sec     21225.01    6429.31   30863.71
    Latency        2.35ms     1.98ms   178.42ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.80MB/s
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
    Reqs/sec     62425.97    4383.26   70513.10
    Latency      800.08us   104.13us     4.84ms
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
    Reqs/sec     18588.50    6459.94   48288.15
    Latency        2.68ms     2.40ms   207.65ms
    HTTP codes:
      1xx - 0, 2xx - 94213, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5787
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5787
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
    Reqs/sec     26031.72    8060.82   57993.22
    Latency        1.91ms     1.79ms   152.07ms
    HTTP codes:
      1xx - 0, 2xx - 96174, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3826
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3810
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 16
    Throughput:     5.73MB/s
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
    Reqs/sec     73551.68    5978.58   84571.35
    Latency      676.90us   228.85us    13.34ms
    HTTP codes:
      1xx - 0, 2xx - 97031, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2969
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2969
    Throughput:    11.30MB/s
  ```


