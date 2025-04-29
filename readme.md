## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `71630` | `6691` | `99172` |
| **85%** | [Hyper Express](#hyper-express) | `60921` | `2500` | `65041` |
| **36%** | [Node (Default)](#node-default) | `25903` | `7372` | `48771` |
| **32%** | [Fastify](#fastify) | `22957` | `6117` | `36661` |
| **29%** | [Hono](#hono) | `20640` | `5519` | `29534` |
| **27%** | [Koa](#koa) | `19155` | `6903` | `52322` |
| **12%** | [Carbon](#carbon) | `8410` | `1520` | `10420` |
| **9%** | [Express](#express) | `6464` | `1038` | `8487` |


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
    Reqs/sec      8825.86    4556.67   55036.02
    Latency        5.66ms     4.17ms   361.76ms
    HTTP codes:
      1xx - 0, 2xx - 93101, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6899
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6899
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
    Reqs/sec      6218.07    1077.15    8368.79
    Latency        8.04ms     3.66ms   347.30ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.78MB/s
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
    Reqs/sec     24639.05    7546.93   36218.32
    Latency        2.03ms     1.96ms   178.17ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.58MB/s
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
    Reqs/sec     20891.19    5773.26   29216.95
    Latency        2.39ms     2.02ms   183.50ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.72MB/s
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
    Reqs/sec     60112.40    3510.44   65746.17
    Latency      830.02us    84.15us     3.02ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.54MB/s
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
    Reqs/sec     18922.54    7441.62   60546.21
    Latency        2.64ms     2.37ms   207.07ms
    HTTP codes:
      1xx - 0, 2xx - 92696, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7304
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7304
    Throughput:     3.97MB/s
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
    Reqs/sec     25640.79    7328.03   51139.77
    Latency        1.95ms     1.89ms   164.45ms
    HTTP codes:
      1xx - 0, 2xx - 97542, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2458
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2458
    Throughput:     5.73MB/s
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
    Reqs/sec     70933.90    4554.78   75935.10
    Latency      702.14us   163.36us     6.08ms
    HTTP codes:
      1xx - 0, 2xx - 96816, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3184
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3184
    Throughput:    10.87MB/s
  ```


