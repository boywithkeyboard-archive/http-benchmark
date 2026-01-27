## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74770` | `2955` | `82895` |
| **82%** | [Hyper Express](#hyper-express) | `61376` | `3440` | `64374` |
| **33%** | [Node (Default)](#node-default) | `24666` | `7828` | `60878` |
| **30%** | [Fastify](#fastify) | `22443` | `7014` | `34865` |
| **26%** | [Hono](#hono) | `19626` | `5308` | `28645` |
| **24%** | [Koa](#koa) | `17995` | `8549` | `79388` |
| **11%** | [Carbon](#carbon) | `7905` | `1375` | `10091` |
| **9%** | [Express](#express) | `6372` | `1116` | `8336` |


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
    Reqs/sec      8718.58    6656.07   80862.91
    Latency        5.72ms     4.37ms   380.32ms
    HTTP codes:
      1xx - 0, 2xx - 89911, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10089
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10089
    Throughput:     1.78MB/s
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
    Reqs/sec      6293.23    1093.85    8185.49
    Latency        7.94ms     3.90ms   372.80ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.80MB/s
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
    Reqs/sec     24235.92    7761.37   35143.21
    Latency        2.06ms     2.08ms   187.39ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.49MB/s
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
    Reqs/sec     20354.90    5878.85   28322.65
    Latency        2.45ms     2.09ms   186.14ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.60MB/s
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
    Reqs/sec     61346.34    3345.06   63853.03
    Latency      812.40us    70.05us     2.98ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.72MB/s
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
    Reqs/sec     18595.08    8317.84   70430.86
    Latency        2.68ms     2.41ms   214.19ms
    HTTP codes:
      1xx - 0, 2xx - 92518, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7482
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7482
    Throughput:     3.89MB/s
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
    Reqs/sec     25666.27    8776.87   62321.18
    Latency        1.95ms     1.99ms   174.72ms
    HTTP codes:
      1xx - 0, 2xx - 97452, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2548
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2548
    Throughput:     5.73MB/s
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
    Reqs/sec     74319.88    2596.71   79658.57
    Latency      670.40us   137.06us     6.76ms
    HTTP codes:
      1xx - 0, 2xx - 97049, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2951
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2951
    Throughput:    11.42MB/s
  ```


