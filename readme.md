## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `113761` | `5449` | `123180` |
| **80%** | [Hyper Express](#hyper-express) | `90964` | `5865` | `94502` |
| **32%** | [Node (Default)](#node-default) | `36270` | `10615` | `93402` |
| **30%** | [Fastify](#fastify) | `34115` | `7813` | `45560` |
| **26%** | [Hono](#hono) | `29720` | `7672` | `45716` |
| **26%** | [Koa](#koa) | `29497` | `13404` | `98751` |
| **10%** | [Carbon](#carbon) | `11546` | `2271` | `15827` |
| **7%** | [Express](#express) | `8139` | `1540` | `11421` |


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
    Reqs/sec     12877.18   11458.06  114530.53
    Latency        3.87ms     4.12ms   341.80ms
    HTTP codes:
      1xx - 0, 2xx - 86937, 3xx - 0, 4xx - 0, 5xx - 0
      others - 13063
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 13063
    Throughput:     2.54MB/s
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
    Reqs/sec      9323.00    9751.23   97125.44
    Latency        5.35ms     3.49ms   320.54ms
    HTTP codes:
      1xx - 0, 2xx - 86746, 3xx - 0, 4xx - 0, 5xx - 0
      others - 13254
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 13238
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 16
    Throughput:     2.32MB/s
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
    Reqs/sec     38371.02   17628.04  112966.77
    Latency        1.30ms     1.65ms   132.19ms
    HTTP codes:
      1xx - 0, 2xx - 79476, 3xx - 0, 4xx - 0, 5xx - 0
      others - 20524
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 20493
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 31
    Throughput:     6.92MB/s
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
    Reqs/sec     33119.23   17240.72  117183.76
    Latency        1.51ms     1.80ms   156.17ms
    HTTP codes:
      1xx - 0, 2xx - 84956, 3xx - 0, 4xx - 0, 5xx - 0
      others - 15044
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 15044
    Throughput:     6.37MB/s
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
    Reqs/sec     93129.22    6824.52  121490.86
    Latency      532.88us   230.03us     8.96ms
    HTTP codes:
      1xx - 0, 2xx - 89365, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10635
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10635
    Throughput:    11.83MB/s
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
    Reqs/sec     31487.30   15334.44  121113.61
    Latency        1.58ms     1.81ms   152.87ms
    HTTP codes:
      1xx - 0, 2xx - 88280, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11720
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11720
    Throughput:     6.28MB/s
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
    Reqs/sec     36197.69   10179.30   97852.10
    Latency        1.38ms     1.51ms   126.71ms
    HTTP codes:
      1xx - 0, 2xx - 96274, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3726
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3726
    Throughput:     7.98MB/s
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
    Reqs/sec    112572.79    5526.85  122505.95
    Latency      441.45us   159.83us     8.03ms
    HTTP codes:
      1xx - 0, 2xx - 95535, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4465
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4465
    Throughput:    17.01MB/s
  ```


