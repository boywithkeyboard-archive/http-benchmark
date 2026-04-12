## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73903` | `4657` | `85026` |
| **83%** | [Hyper Express](#hyper-express) | `61581` | `4932` | `68364` |
| **30%** | [Fastify](#fastify) | `22537` | `6557` | `37296` |
| **30%** | [Node (Default)](#node-default) | `22413` | `8706` | `79809` |
| **30%** | [Hono](#hono) | `22406` | `6343` | `30319` |
| **25%** | [Koa](#koa) | `18217` | `7734` | `65521` |
| **10%** | [Carbon](#carbon) | `7495` | `1217` | `10179` |
| **9%** | [Express](#express) | `6341` | `1078` | `8212` |


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
    Reqs/sec      8251.39    6176.21   79972.98
    Latency        6.05ms     4.51ms   386.02ms
    HTTP codes:
      1xx - 0, 2xx - 90308, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9692
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9692
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
    Reqs/sec      6270.07    1048.87    8127.58
    Latency        7.97ms     3.92ms   375.16ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.79MB/s
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
    Reqs/sec     21058.35    6100.46   36413.01
    Latency        2.37ms     2.00ms   179.71ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.78MB/s
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
    Reqs/sec     21934.64    6225.35   29981.99
    Latency        2.28ms     2.09ms   185.66ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.96MB/s
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
    Reqs/sec     61930.41    3550.16   66293.54
    Latency      805.20us    83.94us     3.27ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.80MB/s
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
    Reqs/sec     19473.49    9224.97   75812.15
    Latency        2.56ms     2.46ms   213.81ms
    HTTP codes:
      1xx - 0, 2xx - 91520, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8480
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8480
    Throughput:     4.03MB/s
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
    Reqs/sec     22930.27    7141.96   72994.50
    Latency        2.17ms     2.10ms   177.79ms
    HTTP codes:
      1xx - 0, 2xx - 96491, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3509
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3509
    Throughput:     5.07MB/s
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
    Reqs/sec     75384.82    4892.98   92248.16
    Latency      660.61us   150.81us     6.31ms
    HTTP codes:
      1xx - 0, 2xx - 96894, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3106
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3106
    Throughput:    11.57MB/s
  ```


