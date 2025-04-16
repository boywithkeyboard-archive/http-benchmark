## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74967` | `5129` | `82665` |
| **82%** | [Hyper Express](#hyper-express) | `61808` | `3473` | `65687` |
| **35%** | [Node (Default)](#node-default) | `26323` | `7674` | `60149` |
| **34%** | [Fastify](#fastify) | `25154` | `7633` | `37446` |
| **28%** | [Hono](#hono) | `21216` | `6227` | `30834` |
| **26%** | [Koa](#koa) | `19537` | `7263` | `56349` |
| **11%** | [Carbon](#carbon) | `8203` | `1378` | `10438` |
| **9%** | [Express](#express) | `6588` | `1089` | `8530` |


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
    Reqs/sec      8679.39    3344.56   47193.06
    Latency        5.75ms     4.25ms   367.38ms
    HTTP codes:
      1xx - 0, 2xx - 95665, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4335
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4335
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
    Reqs/sec      6915.42    4016.57   48620.16
    Latency        7.22ms     3.96ms   359.90ms
    HTTP codes:
      1xx - 0, 2xx - 93000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7000
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7000
    Throughput:     1.84MB/s
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
    Reqs/sec     25429.51    8085.09   37645.85
    Latency        1.96ms     2.05ms   182.16ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.77MB/s
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
    Reqs/sec     19991.00    5583.39   29803.83
    Latency        2.50ms     2.06ms   184.02ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.51MB/s
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
    Reqs/sec     60985.01    3711.83   65586.98
    Latency      817.42us    75.12us     4.31ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.66MB/s
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
    Reqs/sec     17912.82    5990.58   50649.55
    Latency        2.79ms     2.31ms   206.38ms
    HTTP codes:
      1xx - 0, 2xx - 94840, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5160
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5160
    Throughput:     3.84MB/s
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
    Reqs/sec     25700.71    7538.65   55821.68
    Latency        1.94ms     2.00ms   170.94ms
    HTTP codes:
      1xx - 0, 2xx - 97179, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2821
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2821
    Throughput:     5.71MB/s
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
    Reqs/sec     74136.34    5828.78   82352.43
    Latency      672.03us   176.50us     8.71ms
    HTTP codes:
      1xx - 0, 2xx - 98252, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1748
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1748
    Throughput:    11.52MB/s
  ```


