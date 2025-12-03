## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75148` | `3685` | `82361` |
| **84%** | [Hyper Express](#hyper-express) | `62965` | `4078` | `66795` |
| **37%** | [Node (Default)](#node-default) | `28101` | `9598` | `79377` |
| **32%** | [Fastify](#fastify) | `24420` | `7506` | `36356` |
| **27%** | [Hono](#hono) | `20633` | `5631` | `29550` |
| **25%** | [Koa](#koa) | `18505` | `8470` | `82754` |
| **11%** | [Carbon](#carbon) | `8450` | `1521` | `10755` |
| **9%** | [Express](#express) | `6608` | `1134` | `8478` |


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
    Reqs/sec      9062.81    6686.56   78635.33
    Latency        5.51ms     4.24ms   368.54ms
    HTTP codes:
      1xx - 0, 2xx - 90345, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9655
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9655
    Throughput:     1.86MB/s
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
    Reqs/sec      7142.18    6460.10   70766.74
    Latency        6.99ms     3.96ms   363.79ms
    HTTP codes:
      1xx - 0, 2xx - 88828, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11172
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11172
    Throughput:     1.82MB/s
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
    Reqs/sec     24201.67    7197.81   35591.04
    Latency        2.06ms     1.92ms   173.92ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.50MB/s
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
    Reqs/sec     20814.96    5538.00   30282.65
    Latency        2.40ms     2.01ms   180.13ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.70MB/s
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
    Reqs/sec     62937.10    3916.09   68224.93
    Latency      792.33us    70.91us     2.69ms
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
    Reqs/sec     19124.23    8781.18   74726.03
    Latency        2.61ms     2.44ms   213.86ms
    HTTP codes:
      1xx - 0, 2xx - 91827, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8173
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8173
    Throughput:     3.98MB/s
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
    Reqs/sec     25186.55    7862.79   77746.66
    Latency        1.98ms     1.96ms   170.18ms
    HTTP codes:
      1xx - 0, 2xx - 96707, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3293
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3293
    Throughput:     5.58MB/s
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
    Reqs/sec     75707.60    2279.09   84355.32
    Latency      657.91us   125.94us     7.20ms
    HTTP codes:
      1xx - 0, 2xx - 97415, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2585
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2585
    Throughput:    11.67MB/s
  ```


