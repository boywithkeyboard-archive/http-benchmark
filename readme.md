## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `70157` | `4626` | `88365` |
| **83%** | [Hyper Express](#hyper-express) | `58518` | `3286` | `69411` |
| **31%** | [Hono](#hono) | `21542` | `6658` | `31406` |
| **30%** | [Fastify](#fastify) | `20964` | `5459` | `36600` |
| **29%** | [Node (Default)](#node-default) | `20673` | `5557` | `65212` |
| **27%** | [Koa](#koa) | `19166` | `8603` | `68719` |
| **11%** | [Carbon](#carbon) | `7559` | `1441` | `10407` |
| **9%** | [Express](#express) | `6197` | `1072` | `8417` |


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
    Reqs/sec      7867.32    4845.13   72896.65
    Latency        6.34ms     4.58ms   386.80ms
    HTTP codes:
      1xx - 0, 2xx - 93317, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6683
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6683
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
    Reqs/sec      6423.26    1210.58    8365.18
    Latency        7.78ms     3.91ms   373.15ms
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
    Reqs/sec     20635.30    5478.43   36938.20
    Latency        2.42ms     1.95ms   174.97ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.69MB/s
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
    Reqs/sec     21375.98    7053.76   31740.96
    Latency        2.34ms     2.33ms   206.51ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.83MB/s
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
    Reqs/sec     58357.74    3578.48   65182.34
    Latency        0.85ms    98.47us     3.30ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.29MB/s
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
    Reqs/sec     17843.71    9081.57   68946.13
    Latency        2.79ms     2.36ms   209.07ms
    HTTP codes:
      1xx - 0, 2xx - 89946, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10054
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10054
    Throughput:     3.64MB/s
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
    Reqs/sec     20220.69    6384.72   78824.83
    Latency        2.47ms     1.92ms   163.74ms
    HTTP codes:
      1xx - 0, 2xx - 95076, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4924
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4924
    Throughput:     4.40MB/s
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
    Reqs/sec     70117.97    4236.85   80102.35
    Latency      709.13us   194.56us    13.32ms
    HTTP codes:
      1xx - 0, 2xx - 96459, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3541
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3541
    Throughput:    10.71MB/s
  ```


