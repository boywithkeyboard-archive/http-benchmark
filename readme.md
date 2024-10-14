## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `76767` | `6660` | `84850` |
| **79%** | [Hyper Express](#hyper-express) | `60805` | `4442` | `67370` |
| **33%** | [Node (Default)](#node-default) | `25241` | `6780` | `51537` |
| **32%** | [Fastify](#fastify) | `24360` | `7403` | `35494` |
| **28%** | [Hono](#hono) | `21192` | `5851` | `28993` |
| **24%** | [Koa](#koa) | `18045` | `6089` | `44880` |
| **11%** | [Carbon](#carbon) | `8127` | `1422` | `10429` |
| **8%** | [Express](#express) | `6373` | `1009` | `8406` |


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
    Reqs/sec      8551.01    3374.86   45501.75
    Latency        5.81ms     4.28ms   379.28ms
    HTTP codes:
      1xx - 0, 2xx - 95416, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4584
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4584
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
    Reqs/sec      6358.05     981.14    8359.01
    Latency        7.86ms     3.63ms   348.28ms
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
    Reqs/sec     24160.84    6984.84   35560.21
    Latency        2.07ms     1.96ms   179.28ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.48MB/s
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
    Reqs/sec     20936.42    5868.63   29548.60
    Latency        2.39ms     2.06ms   183.99ms
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
    Reqs/sec     62637.45    4116.87   88488.82
    Latency      795.87us    67.17us     2.79ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.90MB/s
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
    Reqs/sec     18057.38    6373.25   50866.60
    Latency        2.74ms     2.39ms   207.64ms
    HTTP codes:
      1xx - 0, 2xx - 93821, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6179
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6179
    Throughput:     3.86MB/s
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
    Reqs/sec     25287.56    7228.28   54482.47
    Latency        1.97ms     1.93ms   166.76ms
    HTTP codes:
      1xx - 0, 2xx - 97339, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2661
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2661
    Throughput:     5.65MB/s
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
    Reqs/sec     77018.64    7473.61   87375.15
    Latency      647.30us   195.32us    10.21ms
    HTTP codes:
      1xx - 0, 2xx - 98134, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1866
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1866
    Throughput:    11.95MB/s
  ```


