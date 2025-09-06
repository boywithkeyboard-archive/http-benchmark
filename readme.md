## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74708` | `2430` | `77655` |
| **83%** | [Hyper Express](#hyper-express) | `62291` | `4045` | `65501` |
| **35%** | [Node (Default)](#node-default) | `26418` | `8694` | `66147` |
| **33%** | [Fastify](#fastify) | `24293` | `7783` | `35434` |
| **28%** | [Hono](#hono) | `20988` | `6182` | `29480` |
| **24%** | [Koa](#koa) | `17846` | `7736` | `66476` |
| **11%** | [Carbon](#carbon) | `8147` | `1454` | `10341` |
| **8%** | [Express](#express) | `6292` | `1031` | `8275` |


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
    Reqs/sec      9019.31    6846.37   73155.43
    Latency        5.53ms     4.48ms   388.55ms
    HTTP codes:
      1xx - 0, 2xx - 89256, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10744
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10744
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
    Reqs/sec      6237.61    1006.96    8268.40
    Latency        8.01ms     3.74ms   360.85ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.78MB/s
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
    Reqs/sec     24311.95    7750.92   35673.41
    Latency        2.06ms     2.04ms   183.07ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.51MB/s
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
    Reqs/sec     20438.83    5899.58   29488.19
    Latency        2.44ms     2.09ms   187.90ms
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
    Reqs/sec     62915.22    3353.82   65782.03
    Latency      792.45us    73.86us     3.24ms
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
    Reqs/sec     18300.81    7219.13   64525.20
    Latency        2.73ms     2.38ms   209.42ms
    HTTP codes:
      1xx - 0, 2xx - 93219, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6781
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6781
    Throughput:     3.86MB/s
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
    Reqs/sec     25892.78    9007.69   82759.90
    Latency        1.93ms     1.98ms   171.76ms
    HTTP codes:
      1xx - 0, 2xx - 95816, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4184
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4184
    Throughput:     5.68MB/s
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
    Reqs/sec     74161.07    2690.99   78866.23
    Latency      671.98us   125.11us     7.24ms
    HTTP codes:
      1xx - 0, 2xx - 97555, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2445
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2445
    Throughput:    11.45MB/s
  ```


