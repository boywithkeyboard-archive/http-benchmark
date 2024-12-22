## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75046` | `5945` | `81508` |
| **84%** | [Hyper Express](#hyper-express) | `62901` | `3381` | `71102` |
| **32%** | [Node (Default)](#node-default) | `23726` | `6186` | `39564` |
| **31%** | [Fastify](#fastify) | `23405` | `6575` | `34828` |
| **26%** | [Hono](#hono) | `19394` | `4957` | `29074` |
| **22%** | [Koa](#koa) | `16695` | `4641` | `40487` |
| **11%** | [Carbon](#carbon) | `8110` | `1415` | `10361` |
| **8%** | [Express](#express) | `6366` | `1014` | `8469` |


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
    Reqs/sec      8472.09    3794.49   56062.71
    Latency        5.89ms     4.26ms   372.44ms
    HTTP codes:
      1xx - 0, 2xx - 94792, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5208
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5208
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
    Reqs/sec      6478.79    1054.50    8430.05
    Latency        7.71ms     3.70ms   352.05ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     22224.92    5881.83   34735.02
    Latency        2.25ms     1.99ms   182.62ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.04MB/s
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
    Reqs/sec     19524.58    5153.15   28815.07
    Latency        2.56ms     1.99ms   181.65ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.41MB/s
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
    Reqs/sec     63113.57    3908.68   72486.91
    Latency      790.95us    73.88us     5.49ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.95MB/s
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
    Reqs/sec     17006.54    5433.64   44411.25
    Latency        2.93ms     2.40ms   207.62ms
    HTTP codes:
      1xx - 0, 2xx - 95065, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4935
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4935
    Throughput:     3.66MB/s
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
    Reqs/sec     24372.26    6725.85   50503.43
    Latency        2.05ms     1.92ms   165.62ms
    HTTP codes:
      1xx - 0, 2xx - 97461, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2539
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2539
    Throughput:     5.43MB/s
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
    Reqs/sec     73815.69    5604.44   85244.83
    Latency      670.93us   246.93us    13.80ms
    HTTP codes:
      1xx - 0, 2xx - 96455, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3545
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3545
    Throughput:    11.27MB/s
  ```


