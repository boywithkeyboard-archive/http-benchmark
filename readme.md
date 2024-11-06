## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75958` | `6280` | `92762` |
| **84%** | [Hyper Express](#hyper-express) | `63494` | `5227` | `91038` |
| **34%** | [Node (Default)](#node-default) | `25505` | `6921` | `43963` |
| **33%** | [Fastify](#fastify) | `25346` | `7742` | `36926` |
| **28%** | [Hono](#hono) | `21116` | `6207` | `30502` |
| **24%** | [Koa](#koa) | `18222` | `5903` | `40004` |
| **11%** | [Carbon](#carbon) | `8126` | `1301` | `10519` |
| **9%** | [Express](#express) | `6606` | `1025` | `8555` |


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
    Reqs/sec      8719.12    4557.58   61620.18
    Latency        5.72ms     4.20ms   364.86ms
    HTTP codes:
      1xx - 0, 2xx - 93331, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6669
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6669
    Throughput:     1.85MB/s
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
    Reqs/sec      6688.78    1050.83    8637.03
    Latency        7.47ms     3.43ms   329.23ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.91MB/s
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
    Reqs/sec     24338.39    7248.61   36025.49
    Latency        2.05ms     1.97ms   177.54ms
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
    Reqs/sec     20954.45    5372.51   29937.24
    Latency        2.38ms     1.91ms   173.31ms
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
    Reqs/sec     63918.94    4071.92   73550.95
    Latency      780.31us    72.50us     4.59ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.08MB/s
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
    Reqs/sec     17795.25    6179.02   52163.32
    Latency        2.80ms     2.34ms   203.99ms
    HTTP codes:
      1xx - 0, 2xx - 94162, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5838
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5838
    Throughput:     3.79MB/s
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
    Reqs/sec     25795.24    7281.43   45958.55
    Latency        1.93ms     1.89ms   162.21ms
    HTTP codes:
      1xx - 0, 2xx - 97741, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2259
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2259
    Throughput:     5.78MB/s
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
    Reqs/sec     74271.32    6104.69   86375.00
    Latency      670.31us   163.09us     7.79ms
    HTTP codes:
      1xx - 0, 2xx - 98096, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1904
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1904
    Throughput:    11.53MB/s
  ```


