## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72558` | `4189` | `78594` |
| **84%** | [Hyper Express](#hyper-express) | `61068` | `3341` | `63137` |
| **37%** | [Node (Default)](#node-default) | `26739` | `8886` | `57000` |
| **33%** | [Fastify](#fastify) | `23929` | `6837` | `35505` |
| **29%** | [Hono](#hono) | `20773` | `5896` | `30026` |
| **26%** | [Koa](#koa) | `18823` | `7319` | `59663` |
| **11%** | [Carbon](#carbon) | `8200` | `1443` | `10313` |
| **9%** | [Express](#express) | `6449` | `1050` | `8385` |


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
    Reqs/sec      8735.48    4966.11   56350.75
    Latency        5.71ms     4.35ms   376.70ms
    HTTP codes:
      1xx - 0, 2xx - 92167, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7833
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7833
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
    Reqs/sec      6409.99    1024.33    8247.02
    Latency        7.80ms     3.75ms   357.78ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.83MB/s
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
    Reqs/sec     24009.68    7309.77   36171.64
    Latency        2.08ms     2.03ms   182.89ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.45MB/s
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
    Reqs/sec     20923.43    6018.20   29491.61
    Latency        2.39ms     2.07ms   186.00ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.73MB/s
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
    Reqs/sec     61918.96    3461.13   64895.09
    Latency      805.49us    70.60us     2.89ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.79MB/s
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
    Reqs/sec     19174.97    7949.32   63129.50
    Latency        2.60ms     2.38ms   210.22ms
    HTTP codes:
      1xx - 0, 2xx - 91555, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8445
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8445
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
    Reqs/sec     26086.51    7861.95   51177.12
    Latency        1.91ms     2.01ms   172.18ms
    HTTP codes:
      1xx - 0, 2xx - 97547, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2453
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2453
    Throughput:     5.83MB/s
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
    Reqs/sec     72260.28    6848.82   87029.29
    Latency      688.79us   291.95us    16.48ms
    HTTP codes:
      1xx - 0, 2xx - 96753, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3247
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3247
    Throughput:    11.05MB/s
  ```


