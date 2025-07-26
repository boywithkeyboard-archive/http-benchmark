## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74048` | `2554` | `80829` |
| **82%** | [Hyper Express](#hyper-express) | `60425` | `3446` | `64741` |
| **33%** | [Fastify](#fastify) | `24291` | `7433` | `35474` |
| **33%** | [Node (Default)](#node-default) | `24077` | `6864` | `59417` |
| **27%** | [Hono](#hono) | `20156` | `5319` | `29501` |
| **25%** | [Koa](#koa) | `18540` | `8820` | `74458` |
| **11%** | [Carbon](#carbon) | `8412` | `1501` | `10399` |
| **9%** | [Express](#express) | `6518` | `1101` | `8449` |


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
    Reqs/sec      8977.74    7174.43   78163.34
    Latency        5.56ms     4.22ms   365.66ms
    HTTP codes:
      1xx - 0, 2xx - 89199, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10801
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10801
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
    Reqs/sec      6560.32    1110.56    8337.59
    Latency        7.62ms     3.59ms   344.61ms
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
    Reqs/sec     24918.64    7949.95   37231.56
    Latency        2.00ms     2.09ms   183.95ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.65MB/s
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
    Reqs/sec     22467.16    6441.60   30352.37
    Latency        2.22ms     2.04ms   181.81ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.07MB/s
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
    Reqs/sec     62971.87    3673.50   68244.95
    Latency      792.24us    66.54us     2.83ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.94MB/s
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
    Reqs/sec     18987.44    8914.52   79241.71
    Latency        2.62ms     2.44ms   215.57ms
    HTTP codes:
      1xx - 0, 2xx - 91764, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8236
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8236
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
    Reqs/sec     26166.67    8457.25   66030.83
    Latency        1.91ms     1.94ms   164.74ms
    HTTP codes:
      1xx - 0, 2xx - 97070, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2930
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2930
    Throughput:     5.82MB/s
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
    Reqs/sec     74433.68    2903.75   80806.37
    Latency      670.01us   139.25us     5.99ms
    HTTP codes:
      1xx - 0, 2xx - 97526, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2474
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2474
    Throughput:    11.48MB/s
  ```


