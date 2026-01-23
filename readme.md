## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74141` | `2733` | `81562` |
| **82%** | [Hyper Express](#hyper-express) | `61021` | `3649` | `64190` |
| **32%** | [Node (Default)](#node-default) | `23360` | `7111` | `75986` |
| **30%** | [Fastify](#fastify) | `22442` | `7356` | `37639` |
| **28%** | [Hono](#hono) | `20681` | `6492` | `30353` |
| **25%** | [Koa](#koa) | `18527` | `9571` | `82692` |
| **11%** | [Carbon](#carbon) | `8109` | `1518` | `10454` |
| **9%** | [Express](#express) | `6379` | `1141` | `8505` |


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
    Reqs/sec      8581.16    5906.80   80761.05
    Latency        5.82ms     4.28ms   369.99ms
    HTTP codes:
      1xx - 0, 2xx - 91720, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8280
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8280
    Throughput:     1.79MB/s
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
    Reqs/sec      6413.07    1155.68    8430.38
    Latency        7.79ms     3.77ms   360.64ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.83MB/s
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
    Reqs/sec     23225.38    7736.28   35767.84
    Latency        2.15ms     2.02ms   182.81ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.27MB/s
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
    Reqs/sec     20548.00    6255.11   30685.66
    Latency        2.43ms     2.13ms   190.58ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.65MB/s
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
    Reqs/sec     61030.84    2885.74   68282.17
    Latency      818.08us    67.37us     2.95ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.66MB/s
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
    Reqs/sec     19292.60    9338.72   73481.65
    Latency        2.59ms     2.51ms   222.09ms
    HTTP codes:
      1xx - 0, 2xx - 91874, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8126
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8126
    Throughput:     4.01MB/s
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
    Reqs/sec     24037.01    7339.54   61390.06
    Latency        2.07ms     1.96ms   174.18ms
    HTTP codes:
      1xx - 0, 2xx - 97489, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2511
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2511
    Throughput:     5.37MB/s
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
    Reqs/sec     73980.94    3204.35   81392.62
    Latency      673.32us   160.26us     9.55ms
    HTTP codes:
      1xx - 0, 2xx - 97562, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2438
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2438
    Throughput:    11.42MB/s
  ```


