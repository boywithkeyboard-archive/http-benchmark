## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74935` | `3466` | `81978` |
| **83%** | [Hyper Express](#hyper-express) | `62174` | `3660` | `64826` |
| **34%** | [Node (Default)](#node-default) | `25177` | `7691` | `63374` |
| **31%** | [Fastify](#fastify) | `23008` | `6926` | `35386` |
| **28%** | [Hono](#hono) | `20793` | `5860` | `29577` |
| **26%** | [Koa](#koa) | `19173` | `8136` | `66297` |
| **11%** | [Carbon](#carbon) | `7929` | `1404` | `10103` |
| **8%** | [Express](#express) | `6200` | `1037` | `8334` |


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
    Reqs/sec      8700.49    5494.85   64116.49
    Latency        5.73ms     4.48ms   385.94ms
    HTTP codes:
      1xx - 0, 2xx - 91942, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8058
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8058
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
    Reqs/sec      6199.81    1004.02    8251.92
    Latency        8.06ms     3.72ms   359.83ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.77MB/s
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
    Reqs/sec     23088.86    6950.88   34908.93
    Latency        2.16ms     2.05ms   182.75ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.24MB/s
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
    Reqs/sec     20575.74    5593.57   29524.29
    Latency        2.43ms     2.04ms   183.47ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.65MB/s
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
    Reqs/sec     62076.51    4949.89   94341.04
    Latency      808.34us    72.72us     2.74ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.76MB/s
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
    Reqs/sec     18103.68    7452.99   67641.33
    Latency        2.75ms     2.48ms   218.08ms
    HTTP codes:
      1xx - 0, 2xx - 93197, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6803
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6803
    Throughput:     3.82MB/s
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
    Reqs/sec     25691.02    8023.52   72893.99
    Latency        1.94ms     2.03ms   170.94ms
    HTTP codes:
      1xx - 0, 2xx - 96663, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3337
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3337
    Throughput:     5.69MB/s
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
    Reqs/sec     74631.98    3317.25   83339.97
    Latency      667.27us   117.89us     5.10ms
    HTTP codes:
      1xx - 0, 2xx - 97480, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2520
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2520
    Throughput:    11.52MB/s
  ```


