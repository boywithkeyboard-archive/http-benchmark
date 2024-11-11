## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73175` | `5597` | `86302` |
| **86%** | [Hyper Express](#hyper-express) | `62760` | `5022` | `91251` |
| **35%** | [Node (Default)](#node-default) | `25526` | `7850` | `53139` |
| **32%** | [Fastify](#fastify) | `23688` | `6978` | `35466` |
| **27%** | [Hono](#hono) | `20096` | `5386` | `28076` |
| **25%** | [Koa](#koa) | `18445` | `6709` | `61733` |
| **11%** | [Carbon](#carbon) | `8159` | `1380` | `10445` |
| **9%** | [Express](#express) | `6436` | `1048` | `8483` |


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
    Reqs/sec      8889.17    4941.68   64738.55
    Latency        5.63ms     4.21ms   363.15ms
    HTTP codes:
      1xx - 0, 2xx - 93054, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6946
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6946
    Throughput:     1.87MB/s
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
    Reqs/sec      6490.85    1064.85    8568.49
    Latency        7.70ms     3.57ms   341.13ms
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
    Reqs/sec     24344.58    7070.07   34786.46
    Latency        2.05ms     2.01ms   177.49ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.52MB/s
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
    Reqs/sec     20286.43    5536.53   28510.34
    Latency        2.46ms     2.02ms   180.99ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.58MB/s
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
    Reqs/sec     63425.20    4992.51   79003.94
    Latency      788.46us    78.14us     4.03ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.98MB/s
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
    Reqs/sec     17389.52    6185.75   51605.56
    Latency        2.87ms     2.36ms   210.74ms
    HTTP codes:
      1xx - 0, 2xx - 93389, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6611
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6611
    Throughput:     3.68MB/s
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
    Reqs/sec     26236.12    8075.89   64810.00
    Latency        1.90ms     1.84ms   155.24ms
    HTTP codes:
      1xx - 0, 2xx - 95890, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4110
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4110
    Throughput:     5.76MB/s
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
    Reqs/sec     74390.95    6052.18   85013.50
    Latency      666.65us   206.89us    11.90ms
    HTTP codes:
      1xx - 0, 2xx - 97398, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2602
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2602
    Throughput:    11.47MB/s
  ```


