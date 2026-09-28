## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `79020` | `3554` | `87478` |
| **88%** | [Hyper Express](#hyper-express) | `69459` | `3745` | `78867` |
| **45%** | [Node (Default)](#node-default) | `35737` | `10573` | `71274` |
| **41%** | [Fastify](#fastify) | `32729` | `9413` | `52370` |
| **38%** | [Koa](#koa) | `29900` | `11810` | `74501` |
| **36%** | [Hono](#hono) | `28509` | `8290` | `47213` |
| **12%** | [Carbon](#carbon) | `9280` | `2373` | `13469` |
| **9%** | [Express](#express) | `7345` | `1731` | `10539` |


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
    Reqs/sec     10343.24    8176.24   90248.28
    Latency        4.83ms     4.45ms   379.61ms
    HTTP codes:
      1xx - 0, 2xx - 89487, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10513
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10513
    Throughput:     2.10MB/s
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
    Reqs/sec      8203.13    6753.46   73835.60
    Latency        6.08ms     3.83ms   348.96ms
    HTTP codes:
      1xx - 0, 2xx - 89588, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10412
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10412
    Throughput:     2.11MB/s
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
    Reqs/sec     33170.93    9937.45   52623.24
    Latency        1.51ms     1.98ms   177.20ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.52MB/s
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
    Reqs/sec     28937.52    7748.78   47658.04
    Latency        1.73ms     1.94ms   171.23ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     6.52MB/s
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
    Reqs/sec     69467.20    2777.08   71682.69
    Latency      717.92us    68.28us     3.58ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.87MB/s
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
    Reqs/sec     31177.88   14154.80   88352.64
    Latency        1.60ms     2.31ms   200.47ms
    HTTP codes:
      1xx - 0, 2xx - 90000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10000
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10000
    Throughput:     6.34MB/s
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
    Reqs/sec     34766.26   10975.34   90554.98
    Latency        1.43ms     1.89ms   161.08ms
    HTTP codes:
      1xx - 0, 2xx - 94038, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5962
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5962
    Throughput:     7.49MB/s
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
    Reqs/sec     79027.92    2973.78   84116.54
    Latency      629.35us   197.44us    11.86ms
    HTTP codes:
      1xx - 0, 2xx - 95917, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4083
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4083
    Throughput:    12.00MB/s
  ```


