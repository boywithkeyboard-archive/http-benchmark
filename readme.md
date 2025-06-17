## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73216` | `5029` | `77749` |
| **83%** | [Hyper Express](#hyper-express) | `60940` | `3456` | `70015` |
| **39%** | [Node (Default)](#node-default) | `28231` | `9083` | `60462` |
| **32%** | [Fastify](#fastify) | `23585` | `7355` | `36996` |
| **29%** | [Hono](#hono) | `21312` | `6241` | `31178` |
| **25%** | [Koa](#koa) | `18594` | `7303` | `63093` |
| **11%** | [Carbon](#carbon) | `8389` | `1493` | `10488` |
| **9%** | [Express](#express) | `6468` | `1065` | `8515` |


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
    Reqs/sec      8658.27    3903.45   52502.34
    Latency        5.77ms     4.24ms   367.90ms
    HTTP codes:
      1xx - 0, 2xx - 94708, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5292
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5292
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
    Reqs/sec      6920.79    4050.69   53526.39
    Latency        7.22ms     3.98ms   365.01ms
    HTTP codes:
      1xx - 0, 2xx - 93034, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6966
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6966
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
    Reqs/sec     24307.92    7912.13   38677.58
    Latency        2.06ms     2.06ms   183.49ms
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
    Reqs/sec     21743.29    6197.31   31530.71
    Latency        2.30ms     2.06ms   188.48ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.91MB/s
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
    Reqs/sec     62219.35    3606.92   66598.99
    Latency      801.64us    69.01us     3.04ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.84MB/s
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
    Reqs/sec     18373.20    7340.03   59020.92
    Latency        2.72ms     2.44ms   214.24ms
    HTTP codes:
      1xx - 0, 2xx - 92751, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7249
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7249
    Throughput:     3.85MB/s
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
    Reqs/sec     26491.70    7493.41   56383.21
    Latency        1.88ms     1.86ms   163.27ms
    HTTP codes:
      1xx - 0, 2xx - 96788, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3212
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3212
    Throughput:     5.87MB/s
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
    Reqs/sec     73154.90    4343.56   78054.87
    Latency      678.46us   188.83us    11.04ms
    HTTP codes:
      1xx - 0, 2xx - 96371, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3629
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3629
    Throughput:    11.15MB/s
  ```


