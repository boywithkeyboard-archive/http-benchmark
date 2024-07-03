## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73819` | `5794` | `83689` |
| **87%** | [Hyper Express](#hyper-express) | `64296` | `5176` | `95804` |
| **36%** | [Node (Default)](#node-default) | `26477` | `7600` | `39491` |
| **31%** | [Fastify](#fastify) | `23171` | `6918` | `34537` |
| **28%** | [Hono](#hono) | `20580` | `5868` | `29619` |
| **24%** | [Koa](#koa) | `17943` | `6334` | `55640` |
| **11%** | [Carbon](#carbon) | `7973` | `1359` | `10256` |
| **9%** | [Express](#express) | `6379` | `1005` | `8390` |


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
    Reqs/sec      8384.43    4064.46   62572.50
    Latency        5.97ms     4.33ms   374.82ms
    HTTP codes:
      1xx - 0, 2xx - 94376, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5624
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5624
    Throughput:     1.79MB/s
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
    Reqs/sec      6395.84    1030.75    8371.67
    Latency        7.81ms     3.65ms   348.78ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.83MB/s
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
    Reqs/sec     23148.59    7194.24   35285.75
    Latency        2.16ms     1.99ms   179.94ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.25MB/s
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
    Reqs/sec     21802.19    6450.68   29375.18
    Latency        2.29ms     2.02ms   180.65ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.92MB/s
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
    Reqs/sec     62850.43    4001.21   68313.81
    Latency      792.74us    72.81us     3.86ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.93MB/s
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
    Reqs/sec     17206.17    5798.25   51554.65
    Latency        2.90ms     2.36ms   218.31ms
    HTTP codes:
      1xx - 0, 2xx - 94812, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5188
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5188
    Throughput:     3.69MB/s
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
    Reqs/sec     24965.60    7397.59   43415.71
    Latency        2.00ms     2.06ms   173.47ms
    HTTP codes:
      1xx - 0, 2xx - 98217, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1783
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1783
    Throughput:     5.61MB/s
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
    Reqs/sec     73579.68    8552.73   89487.94
    Latency      676.50us   304.90us    19.30ms
    HTTP codes:
      1xx - 0, 2xx - 96968, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3032
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3031
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 1
    Throughput:    11.27MB/s
  ```


