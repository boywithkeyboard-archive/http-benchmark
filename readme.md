## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74893` | `3199` | `86949` |
| **84%** | [Hyper Express](#hyper-express) | `62871` | `3487` | `65351` |
| **35%** | [Node (Default)](#node-default) | `26087` | `8567` | `66359` |
| **32%** | [Fastify](#fastify) | `23874` | `7087` | `36050` |
| **28%** | [Hono](#hono) | `21316` | `5940` | `29695` |
| **27%** | [Koa](#koa) | `20019` | `9046` | `72604` |
| **11%** | [Carbon](#carbon) | `8326` | `1429` | `10460` |
| **9%** | [Express](#express) | `6476` | `1061` | `8475` |


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
    Reqs/sec      8961.09    5397.34   68552.66
    Latency        5.57ms     4.33ms   372.34ms
    HTTP codes:
      1xx - 0, 2xx - 92959, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7041
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7041
    Throughput:     1.89MB/s
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
    Reqs/sec      7509.68    7238.16   77599.15
    Latency        6.65ms     3.91ms   357.75ms
    HTTP codes:
      1xx - 0, 2xx - 87324, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12676
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12670
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 6
    Throughput:     1.88MB/s
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
    Reqs/sec     23883.09    7154.08   35872.12
    Latency        2.09ms     2.04ms   182.42ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.42MB/s
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
    Reqs/sec     20646.05    5682.47   29084.04
    Latency        2.42ms     2.02ms   180.82ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.66MB/s
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
    Reqs/sec     63705.99    4003.67   68017.39
    Latency      778.64us    74.80us     3.73ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.10MB/s
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
    Reqs/sec     18625.26    7766.89   65546.77
    Latency        2.68ms     2.51ms   218.30ms
    HTTP codes:
      1xx - 0, 2xx - 92412, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7588
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7588
    Throughput:     3.89MB/s
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
    Reqs/sec     26175.96    8445.26   79911.58
    Latency        1.90ms     1.92ms   161.33ms
    HTTP codes:
      1xx - 0, 2xx - 96437, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3563
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3563
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
    Reqs/sec     75245.43    2987.39   80426.28
    Latency      662.53us   135.74us     5.95ms
    HTTP codes:
      1xx - 0, 2xx - 97476, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2524
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2524
    Throughput:    11.61MB/s
  ```


