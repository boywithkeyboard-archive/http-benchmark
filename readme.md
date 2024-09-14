## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72967` | `5664` | `84870` |
| **84%** | [Hyper Express](#hyper-express) | `61395` | `3630` | `64950` |
| **36%** | [Node (Default)](#node-default) | `26012` | `7671` | `48220` |
| **33%** | [Fastify](#fastify) | `24162` | `7553` | `36527` |
| **29%** | [Hono](#hono) | `21053` | `6194` | `29912` |
| **26%** | [Koa](#koa) | `19125` | `7248` | `67993` |
| **11%** | [Carbon](#carbon) | `8263` | `1412` | `10544` |
| **9%** | [Express](#express) | `6506` | `1043` | `8478` |


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
    Reqs/sec      8802.04    4236.80   53657.17
    Latency        5.67ms     4.13ms   357.34ms
    HTTP codes:
      1xx - 0, 2xx - 94168, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5832
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5832
    Throughput:     1.88MB/s
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
    Reqs/sec      6523.49    1003.61    8420.47
    Latency        7.66ms     3.47ms   339.26ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.87MB/s
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
    Reqs/sec     24528.36    7739.77   35479.27
    Latency        2.04ms     1.99ms   178.80ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.56MB/s
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
    Reqs/sec     21480.34    6264.81   30048.91
    Latency        2.33ms     1.96ms   172.84ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.85MB/s
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
    Reqs/sec     61374.29    3471.45   63769.59
    Latency      812.41us    62.82us     3.10ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.72MB/s
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
    Reqs/sec     17828.49    6361.87   52624.70
    Latency        2.80ms     2.29ms   201.19ms
    HTTP codes:
      1xx - 0, 2xx - 94442, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5558
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5558
    Throughput:     3.80MB/s
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
    Reqs/sec     24896.38    6917.09   49253.58
    Latency        2.01ms     1.80ms   156.66ms
    HTTP codes:
      1xx - 0, 2xx - 97878, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2122
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2122
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
    Reqs/sec     73372.27    7817.69   87767.91
    Latency      678.34us   393.92us    21.50ms
    HTTP codes:
      1xx - 0, 2xx - 96380, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3620
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3620
    Throughput:    11.18MB/s
  ```


