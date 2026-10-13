## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `102956` | `4251` | `109504` |
| **87%** | [Hyper Express](#hyper-express) | `89099` | `4663` | `92865` |
| **46%** | [Node (Default)](#node-default) | `47654` | `11902` | `93339` |
| **45%** | [Fastify](#fastify) | `46833` | `15289` | `67294` |
| **40%** | [Koa](#koa) | `41056` | `17098` | `96730` |
| **39%** | [Hono](#hono) | `40019` | `11042` | `60982` |
| **12%** | [Carbon](#carbon) | `12146` | `2769` | `17605` |
| **9%** | [Express](#express) | `9420` | `1927` | `13499` |


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
    Reqs/sec     13376.88    8993.25   93001.18
    Latency        3.73ms     3.42ms   291.06ms
    HTTP codes:
      1xx - 0, 2xx - 91360, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8640
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8640
    Throughput:     2.78MB/s
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
    Reqs/sec     10367.26    7960.42   88499.37
    Latency        4.81ms     2.91ms   262.15ms
    HTTP codes:
      1xx - 0, 2xx - 90703, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9297
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9297
    Throughput:     2.69MB/s
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
    Reqs/sec     45108.67   12216.53   67620.29
    Latency        1.11ms     1.32ms   121.54ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    10.24MB/s
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
    Reqs/sec     38548.53   12144.48   61652.39
    Latency        1.29ms     1.71ms   145.98ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.72MB/s
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
    Reqs/sec     89171.36    3639.98   96822.42
    Latency      559.97us    44.17us     2.03ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    12.65MB/s
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
    Reqs/sec     40354.29   16415.17  106616.39
    Latency        1.24ms     1.74ms   151.19ms
    HTTP codes:
      1xx - 0, 2xx - 90818, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9182
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9182
    Throughput:     8.29MB/s
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
    Reqs/sec     47405.97   12038.31   85072.30
    Latency        1.05ms     1.29ms   109.63ms
    HTTP codes:
      1xx - 0, 2xx - 97115, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2885
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2885
    Throughput:    10.54MB/s
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
    Reqs/sec    102428.35    3806.63  109313.54
    Latency      485.32us   177.89us    10.42ms
    HTTP codes:
      1xx - 0, 2xx - 96147, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3853
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3853
    Throughput:    15.59MB/s
  ```


