## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `71000` | `4321` | `82875` |
| **84%** | [Hyper Express](#hyper-express) | `59292` | `3750` | `66026` |
| **30%** | [Hono](#hono) | `21360` | `6764` | `30637` |
| **29%** | [Fastify](#fastify) | `20709` | `5481` | `35981` |
| **29%** | [Node (Default)](#node-default) | `20596` | `5529` | `65040` |
| **27%** | [Koa](#koa) | `18917` | `8744` | `74311` |
| **11%** | [Carbon](#carbon) | `7639` | `1400` | `10501` |
| **9%** | [Express](#express) | `6237` | `1138` | `8435` |


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
    Reqs/sec      8408.54    6972.04   74181.79
    Latency        5.94ms     4.44ms   378.87ms
    HTTP codes:
      1xx - 0, 2xx - 88658, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11342
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11342
    Throughput:     1.69MB/s
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
    Reqs/sec      6020.01    1037.84    8464.30
    Latency        8.30ms     3.91ms   374.91ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.72MB/s
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
    Reqs/sec     20424.02    5146.44   35419.46
    Latency        2.45ms     2.08ms   186.70ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.63MB/s
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
    Reqs/sec     21143.61    6291.51   31025.42
    Latency        2.36ms     2.18ms   197.89ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.78MB/s
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
    Reqs/sec     59368.39    3359.67   67067.19
    Latency      840.01us   105.17us     4.29ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.43MB/s
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
    Reqs/sec     19855.62    9272.24   79419.61
    Latency        2.51ms     2.44ms   213.18ms
    HTTP codes:
      1xx - 0, 2xx - 92037, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7963
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7963
    Throughput:     4.13MB/s
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
    Reqs/sec     20684.09    5476.95   64605.00
    Latency        2.41ms     2.01ms   169.88ms
    HTTP codes:
      1xx - 0, 2xx - 96466, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3534
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3534
    Throughput:     4.57MB/s
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
    Reqs/sec     71684.39    5701.60   87087.67
    Latency      694.55us   146.88us     6.19ms
    HTTP codes:
      1xx - 0, 2xx - 96462, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3538
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3526
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 12
    Throughput:    10.95MB/s
  ```


