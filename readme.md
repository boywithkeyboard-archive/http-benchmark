## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72636` | `5954` | `83009` |
| **87%** | [Hyper Express](#hyper-express) | `63344` | `4139` | `70695` |
| **36%** | [Node (Default)](#node-default) | `26278` | `6929` | `52227` |
| **33%** | [Fastify](#fastify) | `23865` | `6642` | `35763` |
| **28%** | [Hono](#hono) | `20552` | `5660` | `29714` |
| **26%** | [Koa](#koa) | `18944` | `6355` | `51702` |
| **11%** | [Carbon](#carbon) | `8270` | `1404` | `10303` |
| **9%** | [Express](#express) | `6503` | `1081` | `8323` |


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
    Reqs/sec      8741.31    4146.37   54029.23
    Latency        5.70ms     4.21ms   367.05ms
    HTTP codes:
      1xx - 0, 2xx - 94425, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5575
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5575
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
    Reqs/sec      7049.82    4934.93   61697.46
    Latency        7.09ms     3.86ms   352.89ms
    HTTP codes:
      1xx - 0, 2xx - 91142, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8858
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8858
    Throughput:     1.84MB/s
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
    Reqs/sec     24769.34    7677.02   36344.96
    Latency        2.02ms     2.07ms   187.07ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.61MB/s
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
    Reqs/sec     21329.01    6078.11   31076.02
    Latency        2.34ms     2.05ms   184.71ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.82MB/s
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
    Reqs/sec     61598.34    3691.99   65658.83
    Latency      809.88us    71.17us     3.06ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.75MB/s
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
    Reqs/sec     19415.38    6977.21   55464.82
    Latency        2.57ms     2.48ms   213.20ms
    HTTP codes:
      1xx - 0, 2xx - 94169, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5831
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5831
    Throughput:     4.13MB/s
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
    Reqs/sec     28974.63    9026.17   48552.98
    Latency        1.72ms     1.81ms   155.68ms
    HTTP codes:
      1xx - 0, 2xx - 97958, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2042
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2042
    Throughput:     6.51MB/s
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
    Reqs/sec     73528.00    4863.79   80151.82
    Latency      677.60us   177.63us     6.80ms
    HTTP codes:
      1xx - 0, 2xx - 96867, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3133
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3133
    Throughput:    11.27MB/s
  ```


