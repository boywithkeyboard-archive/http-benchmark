## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75527` | `3133` | `82923` |
| **82%** | [Hyper Express](#hyper-express) | `62024` | `3867` | `65706` |
| **34%** | [Node (Default)](#node-default) | `25637` | `7889` | `57389` |
| **31%** | [Fastify](#fastify) | `23639` | `7244` | `35778` |
| **28%** | [Hono](#hono) | `20836` | `5617` | `29538` |
| **25%** | [Koa](#koa) | `18769` | `8659` | `72653` |
| **11%** | [Carbon](#carbon) | `8212` | `1424` | `10210` |
| **8%** | [Express](#express) | `6255` | `1013` | `8253` |


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
    Reqs/sec      8768.96    5832.03   80454.21
    Latency        5.69ms     4.30ms   375.34ms
    HTTP codes:
      1xx - 0, 2xx - 91925, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8075
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8075
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
    Reqs/sec      6265.78    1006.68    8227.98
    Latency        7.98ms     3.71ms   355.69ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.79MB/s
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
    Reqs/sec     24080.15    7654.13   36976.04
    Latency        2.08ms     1.94ms   175.69ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.46MB/s
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
    Reqs/sec     20871.50    6116.57   30726.85
    Latency        2.39ms     2.01ms   182.09ms
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
    Reqs/sec     62504.55    3765.10   66826.90
    Latency      797.74us    75.37us     2.99ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.88MB/s
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
    Reqs/sec     18443.20    8096.66   68385.93
    Latency        2.71ms     2.42ms   209.90ms
    HTTP codes:
      1xx - 0, 2xx - 92966, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7034
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7034
    Throughput:     3.88MB/s
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
    Reqs/sec     26322.23    8227.79   70017.50
    Latency        1.90ms     1.97ms   169.53ms
    HTTP codes:
      1xx - 0, 2xx - 96689, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3311
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3311
    Throughput:     5.83MB/s
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
    Reqs/sec     75761.28    3411.26   82361.48
    Latency      657.38us   160.94us     8.33ms
    HTTP codes:
      1xx - 0, 2xx - 96685, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3315
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3315
    Throughput:    11.58MB/s
  ```


