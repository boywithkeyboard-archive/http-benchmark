## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72354` | `4694` | `80953` |
| **85%** | [Hyper Express](#hyper-express) | `61226` | `3579` | `68243` |
| **35%** | [Node (Default)](#node-default) | `25588` | `8274` | `54405` |
| **35%** | [Fastify](#fastify) | `25081` | `8225` | `37690` |
| **29%** | [Hono](#hono) | `20888` | `5902` | `30334` |
| **26%** | [Koa](#koa) | `19095` | `7445` | `57541` |
| **11%** | [Carbon](#carbon) | `8250` | `1417` | `10128` |
| **9%** | [Express](#express) | `6423` | `1032` | `8300` |


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
    Reqs/sec      8918.38    4501.30   58984.44
    Latency        5.60ms     4.21ms   365.74ms
    HTTP codes:
      1xx - 0, 2xx - 92969, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7031
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7031
    Throughput:     1.88MB/s
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
    Reqs/sec      6896.36    3868.52   50344.80
    Latency        7.24ms     3.91ms   358.11ms
    HTTP codes:
      1xx - 0, 2xx - 93460, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6540
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6540
    Throughput:     1.85MB/s
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
    Reqs/sec     23838.14    7131.54   36803.86
    Latency        2.10ms     2.01ms   179.04ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.41MB/s
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
    Reqs/sec     20717.95    5859.11   29385.74
    Latency        2.41ms     2.02ms   181.87ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.68MB/s
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
    Reqs/sec     61335.26    3530.08   63747.63
    Latency      813.19us    77.95us     3.90ms
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
    Reqs/sec     18496.07    6426.94   53817.41
    Latency        2.70ms     2.36ms   209.24ms
    HTTP codes:
      1xx - 0, 2xx - 94047, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5953
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5953
    Throughput:     3.93MB/s
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
    Reqs/sec     26084.77    8344.92   48952.45
    Latency        1.91ms     1.92ms   164.18ms
    HTTP codes:
      1xx - 0, 2xx - 98005, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1995
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1995
    Throughput:     5.85MB/s
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
    Reqs/sec     72885.24    4874.33   87922.98
    Latency      682.46us   172.29us     7.80ms
    HTTP codes:
      1xx - 0, 2xx - 96716, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3284
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3284
    Throughput:    11.16MB/s
  ```


