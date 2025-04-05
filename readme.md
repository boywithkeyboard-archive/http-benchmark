## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74660` | `5951` | `92626` |
| **83%** | [Hyper Express](#hyper-express) | `62258` | `4011` | `69586` |
| **36%** | [Node (Default)](#node-default) | `26943` | `8276` | `49946` |
| **33%** | [Fastify](#fastify) | `24806` | `7710` | `37705` |
| **29%** | [Hono](#hono) | `21399` | `5615` | `30546` |
| **26%** | [Koa](#koa) | `19217` | `6995` | `57949` |
| **11%** | [Carbon](#carbon) | `8405` | `1415` | `10354` |
| **9%** | [Express](#express) | `6531` | `1062` | `8340` |


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
    Reqs/sec      8701.84    4301.66   53528.32
    Latency        5.73ms     4.30ms   372.12ms
    HTTP codes:
      1xx - 0, 2xx - 93530, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6470
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6470
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
    Reqs/sec      7129.04    4878.02   56854.40
    Latency        7.00ms     3.87ms   353.05ms
    HTTP codes:
      1xx - 0, 2xx - 90871, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9129
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9129
    Throughput:     1.86MB/s
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
    Reqs/sec     24412.26    7248.29   36496.53
    Latency        2.05ms     1.98ms   179.81ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.54MB/s
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
    Reqs/sec     20781.82    5928.68   29431.23
    Latency        2.41ms     2.04ms   186.28ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.69MB/s
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
    Reqs/sec     62683.86    3953.74   65626.98
    Latency      795.31us    75.48us     3.67ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.91MB/s
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
    Reqs/sec     20850.77    7328.30   56170.93
    Latency        2.39ms     2.31ms   203.67ms
    HTTP codes:
      1xx - 0, 2xx - 93384, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6616
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6616
    Throughput:     4.40MB/s
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
    Reqs/sec     26088.18    7354.78   49150.28
    Latency        1.91ms     1.80ms   157.02ms
    HTTP codes:
      1xx - 0, 2xx - 98060, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1940
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1940
    Throughput:     5.86MB/s
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
    Reqs/sec     74004.77    4460.53   80397.25
    Latency      673.56us   162.80us     6.08ms
    HTTP codes:
      1xx - 0, 2xx - 97274, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2726
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2726
    Throughput:    11.39MB/s
  ```


