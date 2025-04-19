## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `76091` | `5586` | `88217` |
| **84%** | [Hyper Express](#hyper-express) | `63828` | `3729` | `68687` |
| **36%** | [Node (Default)](#node-default) | `27622` | `8480` | `51401` |
| **33%** | [Fastify](#fastify) | `25093` | `7396` | `38471` |
| **28%** | [Hono](#hono) | `21557` | `6105` | `32512` |
| **26%** | [Koa](#koa) | `19765` | `7103` | `59118` |
| **11%** | [Carbon](#carbon) | `8610` | `1459` | `10937` |
| **9%** | [Express](#express) | `6632` | `1044` | `8751` |


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
    Reqs/sec      9122.64    4773.72   59266.47
    Latency        5.47ms     4.08ms   353.51ms
    HTTP codes:
      1xx - 0, 2xx - 93190, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6810
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6810
    Throughput:     1.93MB/s
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
    Reqs/sec      7344.03    4779.77   58139.32
    Latency        6.79ms     3.73ms   340.82ms
    HTTP codes:
      1xx - 0, 2xx - 91424, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8576
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8576
    Throughput:     1.92MB/s
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
    Reqs/sec     25004.49    7076.58   37896.97
    Latency        2.00ms     1.85ms   165.18ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.67MB/s
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
    Reqs/sec     21645.60    6196.13   32237.86
    Latency        2.31ms     1.90ms   171.50ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.88MB/s
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
    Reqs/sec     64137.10    3419.11   70818.26
    Latency      777.27us    65.40us     2.62ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.11MB/s
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
    Reqs/sec     20019.19    7899.39   65579.24
    Latency        2.49ms     2.30ms   201.35ms
    HTTP codes:
      1xx - 0, 2xx - 91502, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8498
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8498
    Throughput:     4.14MB/s
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
    Reqs/sec     27535.62    7947.73   51242.12
    Latency        1.81ms     1.88ms   159.80ms
    HTTP codes:
      1xx - 0, 2xx - 97744, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2256
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2256
    Throughput:     6.17MB/s
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
    Reqs/sec     74418.84    5785.24   82734.93
    Latency      668.96us   165.26us     8.25ms
    HTTP codes:
      1xx - 0, 2xx - 98225, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1775
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1775
    Throughput:    11.58MB/s
  ```


