## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74667` | `5352` | `85423` |
| **85%** | [Hyper Express](#hyper-express) | `63504` | `3020` | `66417` |
| **34%** | [Node (Default)](#node-default) | `25640` | `7000` | `58086` |
| **34%** | [Fastify](#fastify) | `25173` | `7543` | `37750` |
| **29%** | [Hono](#hono) | `21819` | `6118` | `30603` |
| **25%** | [Koa](#koa) | `19022` | `7290` | `56137` |
| **11%** | [Carbon](#carbon) | `8447` | `1501` | `10551` |
| **9%** | [Express](#express) | `6514` | `1066` | `8525` |


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
    Reqs/sec      9020.13    5176.15   55841.52
    Latency        5.53ms     4.19ms   362.65ms
    HTTP codes:
      1xx - 0, 2xx - 92468, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7532
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7532
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
    Reqs/sec      6926.47    4450.11   50629.50
    Latency        7.21ms     3.82ms   350.86ms
    HTTP codes:
      1xx - 0, 2xx - 91863, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8137
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8137
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
    Reqs/sec     24526.19    7513.25   36625.13
    Latency        2.04ms     1.95ms   179.38ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.56MB/s
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
    Reqs/sec     21576.89    6209.03   30258.72
    Latency        2.31ms     2.07ms   184.56ms
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
    Reqs/sec     64636.10    4120.54   70173.52
    Latency      771.45us    69.32us     2.86ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.18MB/s
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
    Reqs/sec     19395.53    6990.17   59215.15
    Latency        2.57ms     2.27ms   198.21ms
    HTTP codes:
      1xx - 0, 2xx - 93156, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6844
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6844
    Throughput:     4.09MB/s
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
    Reqs/sec     27146.31    8387.67   61071.38
    Latency        1.84ms     1.92ms   156.45ms
    HTTP codes:
      1xx - 0, 2xx - 96639, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3361
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3361
    Throughput:     6.01MB/s
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
    Reqs/sec     74946.52    6015.76   84317.44
    Latency      664.15us   176.27us     6.38ms
    HTTP codes:
      1xx - 0, 2xx - 96943, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3057
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3057
    Throughput:    11.50MB/s
  ```


