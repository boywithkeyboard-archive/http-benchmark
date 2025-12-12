## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74777` | `1838` | `77790` |
| **84%** | [Hyper Express](#hyper-express) | `62941` | `3669` | `67051` |
| **35%** | [Node (Default)](#node-default) | `26294` | `8520` | `76653` |
| **32%** | [Fastify](#fastify) | `24179` | `7051` | `35551` |
| **28%** | [Hono](#hono) | `20854` | `5659` | `29100` |
| **26%** | [Koa](#koa) | `19533` | `9038` | `78694` |
| **11%** | [Carbon](#carbon) | `8201` | `1416` | `10409` |
| **9%** | [Express](#express) | `6541` | `1103` | `8434` |


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
    Reqs/sec      9044.60    5974.47   68884.12
    Latency        5.51ms     4.30ms   374.19ms
    HTTP codes:
      1xx - 0, 2xx - 91488, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8512
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8512
    Throughput:     1.88MB/s
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
    Reqs/sec      7207.34    6184.16   67664.36
    Latency        6.92ms     4.05ms   372.86ms
    HTTP codes:
      1xx - 0, 2xx - 89699, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10301
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10301
    Throughput:     1.85MB/s
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
    Reqs/sec     22943.57    6878.59   35455.69
    Latency        2.18ms     2.04ms   185.16ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.21MB/s
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
    Reqs/sec     21050.61    5547.05   29091.98
    Latency        2.37ms     2.05ms   185.21ms
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
    Reqs/sec     63314.48    3541.57   65926.50
    Latency      787.57us    68.74us     2.86ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.99MB/s
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
    Reqs/sec     19180.55    9186.57   80826.57
    Latency        2.60ms     2.47ms   216.75ms
    HTTP codes:
      1xx - 0, 2xx - 91194, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8806
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8806
    Throughput:     3.95MB/s
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
    Reqs/sec     26054.39    8552.74   79771.15
    Latency        1.92ms     1.98ms   166.11ms
    HTTP codes:
      1xx - 0, 2xx - 96433, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3567
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3567
    Throughput:     5.75MB/s
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
    Reqs/sec     75831.98    2284.24   86574.58
    Latency      655.36us   199.03us    13.07ms
    HTTP codes:
      1xx - 0, 2xx - 95096, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4904
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4904
    Throughput:    11.42MB/s
  ```


