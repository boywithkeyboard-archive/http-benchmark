## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73067` | `5985` | `83394` |
| **85%** | [Hyper Express](#hyper-express) | `61910` | `3743` | `83921` |
| **35%** | [Node (Default)](#node-default) | `25272` | `7177` | `47182` |
| **33%** | [Fastify](#fastify) | `23946` | `7033` | `35804` |
| **29%** | [Hono](#hono) | `21373` | `6269` | `30604` |
| **24%** | [Koa](#koa) | `17798` | `6313` | `54483` |
| **11%** | [Carbon](#carbon) | `8320` | `1373` | `10577` |
| **9%** | [Express](#express) | `6547` | `1053` | `8588` |


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
    Reqs/sec      8843.09    3991.98   52196.12
    Latency        5.64ms     4.09ms   356.44ms
    HTTP codes:
      1xx - 0, 2xx - 94682, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5318
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5318
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
    Reqs/sec      6524.19    1050.32    8580.83
    Latency        7.66ms     3.58ms   342.80ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.87MB/s
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
    Reqs/sec     24171.54    7539.09   35939.87
    Latency        2.07ms     2.01ms   181.29ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.49MB/s
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
    Reqs/sec     19857.67    5531.58   30836.82
    Latency        2.52ms     1.88ms   171.79ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.49MB/s
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
    Reqs/sec     61741.59    4241.87   87877.22
    Latency      807.19us    64.71us     3.57ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.77MB/s
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
    Reqs/sec     18490.35    6475.59   57591.94
    Latency        2.70ms     2.34ms   200.11ms
    HTTP codes:
      1xx - 0, 2xx - 95135, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4865
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4865
    Throughput:     3.97MB/s
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
    Reqs/sec     26378.02    8159.69   49586.59
    Latency        1.89ms     1.85ms   159.37ms
    HTTP codes:
      1xx - 0, 2xx - 96277, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3723
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3723
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
    Reqs/sec     72893.97    5285.17   77465.43
    Latency      680.62us   150.41us     9.02ms
    HTTP codes:
      1xx - 0, 2xx - 97658, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2342
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2342
    Throughput:    11.26MB/s
  ```


