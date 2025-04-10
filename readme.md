## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73178` | `5476` | `81437` |
| **85%** | [Hyper Express](#hyper-express) | `62257` | `3182` | `68801` |
| **35%** | [Node (Default)](#node-default) | `25678` | `7881` | `51410` |
| **34%** | [Fastify](#fastify) | `24548` | `7714` | `36794` |
| **29%** | [Hono](#hono) | `20884` | `5201` | `29004` |
| **26%** | [Koa](#koa) | `18871` | `7395` | `54676` |
| **11%** | [Carbon](#carbon) | `8331` | `1493` | `10415` |
| **9%** | [Express](#express) | `6438` | `1048` | `8442` |


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
    Reqs/sec      8811.82    4505.84   57458.37
    Latency        5.66ms     4.33ms   371.97ms
    HTTP codes:
      1xx - 0, 2xx - 92989, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7011
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7011
    Throughput:     1.86MB/s
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
    Reqs/sec      6308.47    1032.60    8464.74
    Latency        7.92ms     3.91ms   371.32ms
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
    Reqs/sec     25588.86    7633.04   36289.64
    Latency        1.95ms     2.00ms   181.61ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.81MB/s
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
    Reqs/sec     20882.19    5874.33   29580.20
    Latency        2.39ms     2.14ms   187.91ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.72MB/s
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
    Reqs/sec     61330.78    3319.86   64934.06
    Latency      812.93us    65.05us     2.71ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.71MB/s
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
    Reqs/sec     18672.88    6933.98   55871.67
    Latency        2.67ms     2.39ms   209.31ms
    HTTP codes:
      1xx - 0, 2xx - 93467, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6533
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6533
    Throughput:     3.95MB/s
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
    Reqs/sec     26792.23    8276.52   55175.44
    Latency        1.86ms     1.95ms   165.69ms
    HTTP codes:
      1xx - 0, 2xx - 96780, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3220
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3220
    Throughput:     5.94MB/s
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
    Reqs/sec     73516.86    6160.11   86255.96
    Latency      679.70us   157.01us     6.20ms
    HTTP codes:
      1xx - 0, 2xx - 98036, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1964
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1964
    Throughput:    11.37MB/s
  ```


