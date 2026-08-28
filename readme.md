## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `157042` | `7318` | `171655` |
| **86%** | [Hyper Express](#hyper-express) | `135108` | `8899` | `140394` |
| **35%** | [Node (Default)](#node-default) | `54350` | `16105` | `146996` |
| **33%** | [Fastify](#fastify) | `51202` | `8972` | `62512` |
| **28%** | [Hono](#hono) | `44666` | `8909` | `59840` |
| **26%** | [Koa](#koa) | `41255` | `18720` | `143919` |
| **12%** | [Carbon](#carbon) | `19138` | `5434` | `29454` |
| **9%** | [Express](#express) | `13978` | `2805` | `19654` |


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
    Reqs/sec     21705.75   15989.48  139787.02
    Latency        2.29ms     3.20ms   260.85ms
    HTTP codes:
      1xx - 0, 2xx - 87617, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12383
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12383
    Throughput:     4.33MB/s
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
    Reqs/sec     15630.45   16554.48  146685.13
    Latency        3.18ms     2.39ms   218.86ms
    HTTP codes:
      1xx - 0, 2xx - 83676, 3xx - 0, 4xx - 0, 5xx - 0
      others - 16324
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 16324
    Throughput:     3.76MB/s
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
    Reqs/sec     62775.96   30633.49  159576.69
    Latency      792.35us     0.90ms    57.44ms
    HTTP codes:
      1xx - 0, 2xx - 71413, 3xx - 0, 4xx - 0, 5xx - 0
      others - 28587
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 28587
    Throughput:    10.19MB/s
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
    Reqs/sec     47886.11   20370.04  151430.22
    Latency        1.04ms     1.05ms    77.99ms
    HTTP codes:
      1xx - 0, 2xx - 87576, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12424
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12424
    Throughput:     9.49MB/s
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
    Reqs/sec    135364.01    7417.68  142363.20
    Latency      366.73us   171.07us     7.04ms
    HTTP codes:
      1xx - 0, 2xx - 89767, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10233
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10233
    Throughput:    17.27MB/s
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
    Reqs/sec     43447.89   21063.66  153822.82
    Latency        1.15ms     1.20ms    94.96ms
    HTTP codes:
      1xx - 0, 2xx - 86516, 3xx - 0, 4xx - 0, 5xx - 0
      others - 13484
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 13484
    Throughput:     8.48MB/s
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
    Reqs/sec     55001.67   14126.53  130681.70
    Latency        0.91ms   807.30us    65.91ms
    HTTP codes:
      1xx - 0, 2xx - 95745, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4255
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4255
    Throughput:    12.06MB/s
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
    Reqs/sec    156709.74    6690.80  164662.73
    Latency      316.60us   121.46us     6.28ms
    HTTP codes:
      1xx - 0, 2xx - 94740, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5260
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5260
    Throughput:    23.49MB/s
  ```


