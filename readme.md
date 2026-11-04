## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `102325` | `4216` | `110895` |
| **86%** | [Hyper Express](#hyper-express) | `87583` | `4080` | `91814` |
| **45%** | [Node (Default)](#node-default) | `46331` | `15347` | `105978` |
| **38%** | [Fastify](#fastify) | `39309` | `12167` | `63530` |
| **30%** | [Koa](#koa) | `30892` | `13311` | `101332` |
| **30%** | [Hono](#hono) | `30874` | `8896` | `55834` |
| **11%** | [Carbon](#carbon) | `11284` | `2341` | `17487` |
| **9%** | [Express](#express) | `9300` | `1800` | `13428` |


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
    Reqs/sec     12995.65   10308.51  104352.51
    Latency        3.84ms     3.58ms   302.97ms
    HTTP codes:
      1xx - 0, 2xx - 89007, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10993
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10993
    Throughput:     2.63MB/s
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
    Reqs/sec     10459.72    9107.53  103808.99
    Latency        4.77ms     3.02ms   276.33ms
    HTTP codes:
      1xx - 0, 2xx - 89149, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10851
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10823
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 28
    Throughput:     2.67MB/s
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
    Reqs/sec     43400.95   14671.76   65809.46
    Latency        1.15ms     1.52ms   130.92ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.82MB/s
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
    Reqs/sec     34689.39   10969.39   56930.58
    Latency        1.44ms     1.48ms   132.08ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.83MB/s
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
    Reqs/sec     87919.73    4700.53   93834.25
    Latency      567.21us    57.05us     3.36ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    12.48MB/s
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
    Reqs/sec     31496.14   15883.07  112237.47
    Latency        1.58ms     1.85ms   162.20ms
    HTTP codes:
      1xx - 0, 2xx - 87928, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12072
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12072
    Throughput:     6.26MB/s
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
    Reqs/sec     45911.79   15381.85  108302.52
    Latency        1.09ms     1.43ms   117.77ms
    HTTP codes:
      1xx - 0, 2xx - 94870, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5130
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5130
    Throughput:     9.95MB/s
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
    Reqs/sec    102683.97    3958.58  118536.88
    Latency      484.50us   135.12us     6.73ms
    HTTP codes:
      1xx - 0, 2xx - 95950, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4050
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4050
    Throughput:    15.59MB/s
  ```


