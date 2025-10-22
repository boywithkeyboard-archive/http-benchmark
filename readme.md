## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75072` | `3039` | `82772` |
| **84%** | [Hyper Express](#hyper-express) | `62742` | `3151` | `66055` |
| **34%** | [Node (Default)](#node-default) | `25272` | `7939` | `60072` |
| **32%** | [Fastify](#fastify) | `24141` | `7362` | `36350` |
| **27%** | [Hono](#hono) | `20187` | `5709` | `30612` |
| **26%** | [Koa](#koa) | `19252` | `8300` | `71930` |
| **11%** | [Carbon](#carbon) | `8532` | `1517` | `10515` |
| **8%** | [Express](#express) | `6373` | `1013` | `8359` |


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
    Reqs/sec      8926.71    5253.02   65069.19
    Latency        5.59ms     4.16ms   360.05ms
    HTTP codes:
      1xx - 0, 2xx - 93037, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6963
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6963
    Throughput:     1.89MB/s
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
    Reqs/sec      7021.95    6008.27   73644.05
    Latency        7.11ms     3.90ms   358.49ms
    HTTP codes:
      1xx - 0, 2xx - 90019, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9981
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9981
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
    Reqs/sec     23376.60    6856.52   35822.33
    Latency        2.14ms     1.99ms   178.97ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.30MB/s
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
    Reqs/sec     21425.31    6354.10   30289.80
    Latency        2.33ms     2.05ms   182.26ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.85MB/s
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
    Reqs/sec     62872.19    3658.77   67685.82
    Latency      792.76us    67.79us     2.92ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.93MB/s
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
    Reqs/sec     19030.60    8807.03   79869.68
    Latency        2.62ms     2.31ms   207.89ms
    HTTP codes:
      1xx - 0, 2xx - 91509, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8491
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8491
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
    Reqs/sec     25466.77    7465.40   62384.39
    Latency        1.96ms     1.86ms   159.91ms
    HTTP codes:
      1xx - 0, 2xx - 97418, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2582
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2582
    Throughput:     5.67MB/s
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
    Reqs/sec     74895.20    2480.06   79917.59
    Latency      664.82us   179.29us     8.59ms
    HTTP codes:
      1xx - 0, 2xx - 96587, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3413
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3413
    Throughput:    11.45MB/s
  ```


