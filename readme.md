## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73943` | `5369` | `82648` |
| **84%** | [Hyper Express](#hyper-express) | `61902` | `3278` | `64396` |
| **36%** | [Node (Default)](#node-default) | `26476` | `8105` | `48239` |
| **33%** | [Fastify](#fastify) | `24673` | `7034` | `37062` |
| **28%** | [Hono](#hono) | `20947` | `6126` | `30733` |
| **25%** | [Koa](#koa) | `18850` | `8503` | `66528` |
| **11%** | [Carbon](#carbon) | `8262` | `1411` | `10380` |
| **9%** | [Express](#express) | `6347` | `957` | `8323` |


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
    Reqs/sec      8624.07    4651.21   58338.17
    Latency        5.79ms     4.37ms   376.62ms
    HTTP codes:
      1xx - 0, 2xx - 92670, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7330
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7330
    Throughput:     1.82MB/s
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
    Reqs/sec      6527.61    1044.10    8423.24
    Latency        7.66ms     3.63ms   346.82ms
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
    Reqs/sec     24366.59    7680.70   37241.21
    Latency        2.05ms     2.02ms   180.79ms
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
    Reqs/sec     21049.30    5933.33   30636.52
    Latency        2.38ms     2.11ms   187.32ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.75MB/s
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
    Reqs/sec     62552.95    3773.59   74430.80
    Latency      798.89us    70.47us     2.67ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.87MB/s
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
    Reqs/sec     19136.41    7517.72   60274.33
    Latency        2.61ms     2.40ms   210.72ms
    HTTP codes:
      1xx - 0, 2xx - 92102, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7898
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7898
    Throughput:     3.99MB/s
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
    Reqs/sec     25495.76    7054.83   56777.18
    Latency        1.96ms     1.86ms   160.70ms
    HTTP codes:
      1xx - 0, 2xx - 96921, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3079
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3079
    Throughput:     5.66MB/s
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
    Reqs/sec     73979.94    4634.23   79977.47
    Latency      673.60us   156.82us     6.62ms
    HTTP codes:
      1xx - 0, 2xx - 97451, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2549
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2549
    Throughput:    11.41MB/s
  ```


