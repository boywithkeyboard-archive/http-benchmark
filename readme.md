## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73213` | `4468` | `80983` |
| **85%** | [Hyper Express](#hyper-express) | `62321` | `2682` | `64829` |
| **35%** | [Node (Default)](#node-default) | `25959` | `8218` | `66012` |
| **34%** | [Fastify](#fastify) | `25026` | `7831` | `35740` |
| **28%** | [Hono](#hono) | `20840` | `6079` | `30440` |
| **26%** | [Koa](#koa) | `18784` | `6532` | `53035` |
| **11%** | [Carbon](#carbon) | `8142` | `1414` | `10367` |
| **9%** | [Express](#express) | `6349` | `1025` | `8459` |


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
    Reqs/sec      9071.71    5661.32   57868.81
    Latency        5.50ms     4.27ms   368.27ms
    HTTP codes:
      1xx - 0, 2xx - 90421, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9579
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9579
    Throughput:     1.86MB/s
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
    Reqs/sec      7236.17    5710.64   58607.85
    Latency        6.90ms     4.00ms   365.14ms
    HTTP codes:
      1xx - 0, 2xx - 88371, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11629
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11629
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
    Reqs/sec     24778.69    7927.65   36717.80
    Latency        2.02ms     2.01ms   180.12ms
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
    Reqs/sec     20768.37    6315.65   30919.88
    Latency        2.41ms     2.01ms   183.22ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.69MB/s
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
    Reqs/sec     61120.36    3496.57   65884.74
    Latency      815.71us    74.89us     3.10ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.68MB/s
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
    Reqs/sec     19112.00    6524.32   50773.94
    Latency        2.61ms     2.35ms   204.94ms
    HTTP codes:
      1xx - 0, 2xx - 94810, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5190
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5190
    Throughput:     4.10MB/s
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
    Reqs/sec     26122.74    7901.20   56505.65
    Latency        1.91ms     1.98ms   170.79ms
    HTTP codes:
      1xx - 0, 2xx - 96394, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3606
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3606
    Throughput:     5.76MB/s
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
    Reqs/sec     73207.42    5003.60   84165.88
    Latency      680.68us   171.76us     6.13ms
    HTTP codes:
      1xx - 0, 2xx - 97025, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2975
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2975
    Throughput:    11.24MB/s
  ```


