## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `127118` | `5369` | `132075` |
| **82%** | [Hyper Express](#hyper-express) | `103822` | `7357` | `107736` |
| **32%** | [Node (Default)](#node-default) | `40638` | `11006` | `105314` |
| **30%** | [Fastify](#fastify) | `38183` | `9949` | `57201` |
| **28%** | [Hono](#hono) | `35478` | `8996` | `50146` |
| **26%** | [Koa](#koa) | `33407` | `15884` | `117014` |
| **10%** | [Carbon](#carbon) | `12659` | `2536` | `16848` |
| **7%** | [Express](#express) | `8972` | `1685` | `11979` |


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
    Reqs/sec     13928.59   11267.79  114965.16
    Latency        3.58ms     3.54ms   303.79ms
    HTTP codes:
      1xx - 0, 2xx - 88174, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11826
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11826
    Throughput:     2.79MB/s
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
    Reqs/sec     10173.48   11388.69  111424.31
    Latency        4.90ms     3.21ms   291.55ms
    HTTP codes:
      1xx - 0, 2xx - 85267, 3xx - 0, 4xx - 0, 5xx - 0
      others - 14733
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 14733
    Throughput:     2.49MB/s
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
    Reqs/sec     37577.48    8870.08   56169.30
    Latency        1.33ms     1.36ms   122.06ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.51MB/s
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
    Reqs/sec     31510.04    6513.53   43894.61
    Latency        1.59ms     1.41ms   126.25ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.12MB/s
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
    Reqs/sec    102665.86    6163.02  106034.13
    Latency      485.12us    42.24us     2.71ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    14.58MB/s
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
    Reqs/sec     33999.87   16725.41  130705.00
    Latency        1.47ms     1.70ms   144.72ms
    HTTP codes:
      1xx - 0, 2xx - 87605, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12395
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12395
    Throughput:     6.73MB/s
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
    Reqs/sec     40599.94   11937.90  107850.65
    Latency        1.23ms     1.26ms    98.54ms
    HTTP codes:
      1xx - 0, 2xx - 95785, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4215
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4215
    Throughput:     8.91MB/s
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
    Reqs/sec    127468.93    5877.64  137885.43
    Latency      389.41us   126.61us     6.79ms
    HTTP codes:
      1xx - 0, 2xx - 96078, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3922
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3922
    Throughput:    19.39MB/s
  ```


