## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73198` | `4155` | `84572` |
| **86%** | [Hyper Express](#hyper-express) | `62800` | `4580` | `70809` |
| **31%** | [Hono](#hono) | `23054` | `6464` | `30752` |
| **31%** | [Fastify](#fastify) | `22389` | `6107` | `37391` |
| **30%** | [Node (Default)](#node-default) | `21925` | `6091` | `66466` |
| **26%** | [Koa](#koa) | `18734` | `8932` | `78915` |
| **10%** | [Carbon](#carbon) | `7400` | `1144` | `10470` |
| **9%** | [Express](#express) | `6294` | `1064` | `8307` |


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
    Reqs/sec      8368.15    6382.84   66741.74
    Latency        5.96ms     4.39ms   376.32ms
    HTTP codes:
      1xx - 0, 2xx - 89488, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10512
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10512
    Throughput:     1.70MB/s
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
    Reqs/sec      6361.89    1083.75    8332.85
    Latency        7.86ms     3.68ms   354.77ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     22155.75    6400.48   35838.75
    Latency        2.26ms     2.11ms   188.58ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.02MB/s
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
    Reqs/sec     21943.05    5816.25   30557.55
    Latency        2.28ms     2.03ms   183.48ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.96MB/s
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
    Reqs/sec     61799.38    3807.13   68613.13
    Latency      806.72us    79.44us     2.88ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.78MB/s
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
    Reqs/sec     17431.04    8895.45   67800.69
    Latency        2.86ms     2.47ms   211.76ms
    HTTP codes:
      1xx - 0, 2xx - 91103, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8897
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8897
    Throughput:     3.59MB/s
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
    Reqs/sec     22233.52    6510.24   73156.26
    Latency        2.25ms     1.99ms   170.03ms
    HTTP codes:
      1xx - 0, 2xx - 96618, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3382
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3382
    Throughput:     4.91MB/s
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
    Reqs/sec     73882.63    4031.66   82781.49
    Latency      673.83us   176.31us     9.89ms
    HTTP codes:
      1xx - 0, 2xx - 96620, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3380
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3380
    Throughput:    11.30MB/s
  ```


