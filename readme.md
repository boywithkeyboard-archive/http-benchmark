## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `68173` | `3838` | `76867` |
| **82%** | [Hyper Express](#hyper-express) | `55826` | `4117` | `61533` |
| **38%** | [Node (Default)](#node-default) | `26034` | `8910` | `62965` |
| **30%** | [Fastify](#fastify) | `20364` | `5982` | `33373` |
| **27%** | [Hono](#hono) | `18689` | `6066` | `27494` |
| **26%** | [Koa](#koa) | `17911` | `8747` | `75137` |
| **11%** | [Carbon](#carbon) | `7761` | `1448` | `10289` |
| **9%** | [Express](#express) | `5972` | `1009` | `8056` |


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
    Reqs/sec      8833.88    6416.56   73543.86
    Latency        5.64ms     4.68ms   397.70ms
    HTTP codes:
      1xx - 0, 2xx - 89801, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10199
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10199
    Throughput:     1.81MB/s
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
    Reqs/sec      6005.81     995.78    8131.66
    Latency        8.32ms     3.91ms   374.79ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.72MB/s
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
    Reqs/sec     23051.44    7328.56   33662.24
    Latency        2.17ms     2.10ms   187.76ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.23MB/s
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
    Reqs/sec     18177.80    5585.78   27457.43
    Latency        2.75ms     2.21ms   195.70ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.11MB/s
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
    Reqs/sec     55079.19    4437.99   62780.70
    Latency        0.91ms   162.41us     5.86ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.82MB/s
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
    Reqs/sec     16969.75    6878.21   59235.63
    Latency        2.94ms     2.39ms   212.06ms
    HTTP codes:
      1xx - 0, 2xx - 93914, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6086
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6086
    Throughput:     3.60MB/s
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
    Reqs/sec     24969.24    7830.75   58730.80
    Latency        2.00ms     1.94ms   168.87ms
    HTTP codes:
      1xx - 0, 2xx - 97288, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2712
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2712
    Throughput:     5.57MB/s
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
    Reqs/sec     68751.42    4968.50   79621.92
    Latency      724.18us   189.58us     8.38ms
    HTTP codes:
      1xx - 0, 2xx - 96528, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3472
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3472
    Throughput:    10.50MB/s
  ```


