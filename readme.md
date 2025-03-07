## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `70977` | `4947` | `79738` |
| **86%** | [Hyper Express](#hyper-express) | `61177` | `3526` | `64049` |
| **38%** | [Node (Default)](#node-default) | `26814` | `8353` | `50886` |
| **35%** | [Fastify](#fastify) | `24537` | `7488` | `35258` |
| **29%** | [Hono](#hono) | `20837` | `5735` | `29420` |
| **26%** | [Koa](#koa) | `18710` | `7422` | `59898` |
| **11%** | [Carbon](#carbon) | `8153` | `1419` | `10175` |
| **9%** | [Express](#express) | `6410` | `1027` | `8466` |


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
    Reqs/sec      8530.36    3899.21   52726.98
    Latency        5.85ms     4.31ms   376.76ms
    HTTP codes:
      1xx - 0, 2xx - 94798, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5202
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5202
    Throughput:     1.84MB/s
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
    Reqs/sec      6516.60    1085.10    8528.23
    Latency        7.67ms     3.71ms   351.88ms
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
    Reqs/sec     24922.82    8056.02   35699.73
    Latency        2.01ms     1.97ms   177.06ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.65MB/s
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
    Reqs/sec     20805.44    5625.89   29554.03
    Latency        2.40ms     2.02ms   181.31ms
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
    Reqs/sec     61118.69    3326.90   63569.43
    Latency      815.69us    73.18us     2.97ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.68MB/s
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
    Reqs/sec     18476.87    7015.44   56114.31
    Latency        2.70ms     2.40ms   206.37ms
    HTTP codes:
      1xx - 0, 2xx - 93423, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6577
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6577
    Throughput:     3.90MB/s
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
    Reqs/sec     27021.48    8228.65   53965.98
    Latency        1.85ms     2.10ms   174.02ms
    HTTP codes:
      1xx - 0, 2xx - 96706, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3294
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3294
    Throughput:     5.98MB/s
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
    Reqs/sec     72490.87    4441.97   79752.05
    Latency      687.51us   153.68us     6.47ms
    HTTP codes:
      1xx - 0, 2xx - 97392, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2608
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2608
    Throughput:    11.17MB/s
  ```


