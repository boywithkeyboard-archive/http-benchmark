## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73332` | `5408` | `80331` |
| **85%** | [Hyper Express](#hyper-express) | `62188` | `3779` | `71964` |
| **35%** | [Node (Default)](#node-default) | `25869` | `7068` | `43858` |
| **32%** | [Fastify](#fastify) | `23625` | `7453` | `35952` |
| **27%** | [Hono](#hono) | `20004` | `5637` | `29680` |
| **24%** | [Koa](#koa) | `17440` | `6650` | `52824` |
| **11%** | [Carbon](#carbon) | `8367` | `1413` | `10695` |
| **9%** | [Express](#express) | `6553` | `1072` | `8491` |


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
    Reqs/sec      8833.47    4091.59   59951.17
    Latency        5.66ms     4.13ms   358.22ms
    HTTP codes:
      1xx - 0, 2xx - 94691, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5309
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5309
    Throughput:     1.90MB/s
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
    Reqs/sec      6489.91    1004.12    8524.54
    Latency        7.70ms     3.60ms   345.09ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.86MB/s
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
    Reqs/sec     23707.69    7386.54   34861.12
    Latency        2.11ms     2.07ms   186.23ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.38MB/s
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
    Reqs/sec     19610.91    5347.76   30230.43
    Latency        2.55ms     1.93ms   175.37ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.43MB/s
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
    Reqs/sec     62555.51    3404.85   66237.64
    Latency      797.17us    72.51us     3.60ms
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
    Reqs/sec     18269.99    6636.25   51850.72
    Latency        2.73ms     2.15ms   191.59ms
    HTTP codes:
      1xx - 0, 2xx - 94075, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5925
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5925
    Throughput:     3.89MB/s
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
    Reqs/sec     24116.06    6660.87   67453.18
    Latency        2.07ms     1.91ms   165.21ms
    HTTP codes:
      1xx - 0, 2xx - 96894, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3106
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3103
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 3
    Throughput:     5.35MB/s
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
    Reqs/sec     73695.00    6818.02   98635.43
    Latency      677.26us   155.10us    11.00ms
    HTTP codes:
      1xx - 0, 2xx - 97430, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2570
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2562
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 8
    Throughput:    11.31MB/s
  ```


