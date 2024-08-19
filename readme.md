## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `71562` | `7700` | `79088` |
| **88%** | [Hyper Express](#hyper-express) | `62651` | `3229` | `70829` |
| **35%** | [Node (Default)](#node-default) | `25350` | `7232` | `50102` |
| **34%** | [Fastify](#fastify) | `24224` | `7323` | `36295` |
| **29%** | [Hono](#hono) | `20754` | `6029` | `30046` |
| **27%** | [Koa](#koa) | `19220` | `7907` | `67544` |
| **12%** | [Carbon](#carbon) | `8341` | `1447` | `10526` |
| **9%** | [Express](#express) | `6468` | `1012` | `8481` |


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
    Reqs/sec      8767.68    4315.84   53417.74
    Latency        5.69ms     4.10ms   350.63ms
    HTTP codes:
      1xx - 0, 2xx - 94175, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5825
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5825
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
    Reqs/sec      6776.93    3522.29   49950.20
    Latency        7.36ms     3.92ms   357.19ms
    HTTP codes:
      1xx - 0, 2xx - 93807, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6193
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6193
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
    Reqs/sec     23652.51    7335.13   35543.49
    Latency        2.11ms     1.97ms   178.47ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.37MB/s
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
    Reqs/sec     20612.49    5747.57   29611.38
    Latency        2.42ms     1.87ms   171.18ms
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
    Reqs/sec     62599.33    4215.43   76117.98
    Latency      798.13us    76.18us     4.67ms
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
    Reqs/sec     18889.48    6166.21   51093.91
    Latency        2.64ms     2.32ms   200.43ms
    HTTP codes:
      1xx - 0, 2xx - 93125, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6875
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6875
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
    Reqs/sec     26916.49    8062.12   57112.33
    Latency        1.85ms     1.92ms   162.19ms
    HTTP codes:
      1xx - 0, 2xx - 97359, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2641
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2640
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 1
    Throughput:     6.00MB/s
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
    Reqs/sec     73735.12    4932.42   82410.82
    Latency      674.05us   193.51us    10.77ms
    HTTP codes:
      1xx - 0, 2xx - 96464, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3536
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3534
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 2
    Throughput:    11.26MB/s
  ```


