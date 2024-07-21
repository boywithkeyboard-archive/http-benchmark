## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72696` | `5593` | `79474` |
| **86%** | [Hyper Express](#hyper-express) | `62609` | `3605` | `66646` |
| **35%** | [Node (Default)](#node-default) | `25368` | `7648` | `54710` |
| **32%** | [Fastify](#fastify) | `23007` | `5984` | `34727` |
| **28%** | [Hono](#hono) | `20264` | `5487` | `29634` |
| **25%** | [Koa](#koa) | `18316` | `5756` | `45930` |
| **11%** | [Carbon](#carbon) | `8234` | `1337` | `10381` |
| **9%** | [Express](#express) | `6506` | `1012` | `8501` |


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
    Reqs/sec      8635.19    3773.32   54714.35
    Latency        5.78ms     4.20ms   362.60ms
    HTTP codes:
      1xx - 0, 2xx - 94898, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5102
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5102
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
    Reqs/sec      6556.21    1010.68    8427.28
    Latency        7.62ms     3.54ms   338.90ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.88MB/s
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
    Reqs/sec     23559.49    7004.77   35518.04
    Latency        2.12ms     1.90ms   170.38ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.34MB/s
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
    Reqs/sec     20439.07    5854.19   30145.34
    Latency        2.44ms     1.96ms   176.45ms
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
    Reqs/sec     62650.18    3143.53   64943.37
    Latency      795.54us    64.25us     5.54ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.90MB/s
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
    Reqs/sec     17854.24    5715.35   45383.90
    Latency        2.79ms     2.26ms   199.47ms
    HTTP codes:
      1xx - 0, 2xx - 95271, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4729
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4729
    Throughput:     3.85MB/s
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
    Reqs/sec     24846.43    7630.41   60294.63
    Latency        2.01ms     1.90ms   163.77ms
    HTTP codes:
      1xx - 0, 2xx - 96910, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3090
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3090
    Throughput:     5.52MB/s
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
    Reqs/sec     76469.41    6534.87   88103.01
    Latency      651.92us   248.45us    19.09ms
    HTTP codes:
      1xx - 0, 2xx - 97656, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2344
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2344
    Throughput:    11.81MB/s
  ```


