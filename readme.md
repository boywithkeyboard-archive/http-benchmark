## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74612` | `3429` | `88472` |
| **83%** | [Hyper Express](#hyper-express) | `61963` | `3542` | `69763` |
| **31%** | [Node (Default)](#node-default) | `23235` | `7267` | `78460` |
| **30%** | [Hono](#hono) | `22104` | `6300` | `30795` |
| **29%** | [Fastify](#fastify) | `21758` | `5990` | `37417` |
| **25%** | [Koa](#koa) | `18430` | `8700` | `71442` |
| **10%** | [Carbon](#carbon) | `7602` | `1308` | `10353` |
| **8%** | [Express](#express) | `6251` | `1049` | `8276` |


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
    Reqs/sec      8168.25    5848.36   72080.86
    Latency        6.11ms     4.35ms   374.29ms
    HTTP codes:
      1xx - 0, 2xx - 90915, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9085
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9085
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
    Reqs/sec      6105.22     969.32    8209.50
    Latency        8.19ms     3.86ms   370.08ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.75MB/s
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
    Reqs/sec     21619.26    6150.42   37065.65
    Latency        2.31ms     1.91ms   175.09ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.91MB/s
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
    Reqs/sec     22894.26    6789.63   30800.46
    Latency        2.18ms     2.13ms   189.16ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.17MB/s
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
    Reqs/sec     62126.33    3690.04   71379.10
    Latency      802.68us    75.01us     2.99ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.83MB/s
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
    Reqs/sec     19226.73    8399.83   66028.71
    Latency        2.59ms     2.35ms   206.12ms
    HTTP codes:
      1xx - 0, 2xx - 93194, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6806
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6806
    Throughput:     4.05MB/s
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
    Reqs/sec     22437.69    6442.96   66336.92
    Latency        2.23ms     2.08ms   178.31ms
    HTTP codes:
      1xx - 0, 2xx - 96702, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3298
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3298
    Throughput:     4.96MB/s
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
    Reqs/sec     73768.23    3817.66   84151.14
    Latency      675.34us   187.41us     8.73ms
    HTTP codes:
      1xx - 0, 2xx - 96782, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3218
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3218
    Throughput:    11.30MB/s
  ```


