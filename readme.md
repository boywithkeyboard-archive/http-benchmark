## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73338` | `4894` | `81002` |
| **85%** | [Hyper Express](#hyper-express) | `62583` | `3879` | `66135` |
| **36%** | [Node (Default)](#node-default) | `26536` | `7537` | `50864` |
| **34%** | [Fastify](#fastify) | `24761` | `7438` | `36103` |
| **28%** | [Hono](#hono) | `20523` | `5579` | `29341` |
| **25%** | [Koa](#koa) | `18408` | `6936` | `52397` |
| **11%** | [Carbon](#carbon) | `8309` | `1456` | `10350` |
| **9%** | [Express](#express) | `6380` | `1004` | `8313` |


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
    Reqs/sec      8832.14    4434.91   60754.93
    Latency        5.65ms     4.43ms   385.82ms
    HTTP codes:
      1xx - 0, 2xx - 92593, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7407
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7407
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
    Reqs/sec      6812.11    4569.44   54401.05
    Latency        7.33ms     3.95ms   359.84ms
    HTTP codes:
      1xx - 0, 2xx - 91595, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8405
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8405
    Throughput:     1.79MB/s
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
    Reqs/sec     24412.02    7177.35   36640.39
    Latency        2.05ms     2.02ms   181.28ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.53MB/s
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
    Reqs/sec     21506.43    5861.80   29543.33
    Latency        2.32ms     1.90ms   171.13ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.86MB/s
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
    Reqs/sec     62303.25    4342.81   72587.13
    Latency      799.63us    81.47us     3.63ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.86MB/s
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
    Reqs/sec     18972.50    6660.32   57956.35
    Latency        2.63ms     2.35ms   206.18ms
    HTTP codes:
      1xx - 0, 2xx - 94157, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5843
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5843
    Throughput:     4.04MB/s
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
    Reqs/sec     26034.56    7827.75   60186.22
    Latency        1.92ms     1.94ms   164.79ms
    HTTP codes:
      1xx - 0, 2xx - 97405, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2595
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2595
    Throughput:     5.81MB/s
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
    Reqs/sec     73594.67    6007.43   86250.27
    Latency      677.04us   194.11us     6.56ms
    HTTP codes:
      1xx - 0, 2xx - 97355, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2645
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2645
    Throughput:    11.34MB/s
  ```


