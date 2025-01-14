## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73793` | `5259` | `85246` |
| **86%** | [Hyper Express](#hyper-express) | `63813` | `3303` | `71929` |
| **36%** | [Node (Default)](#node-default) | `26725` | `8328` | `42230` |
| **33%** | [Fastify](#fastify) | `24321` | `7439` | `36303` |
| **28%** | [Hono](#hono) | `20664` | `5395` | `29864` |
| **23%** | [Koa](#koa) | `17236` | `5743` | `51691` |
| **11%** | [Carbon](#carbon) | `8180` | `1373` | `10469` |
| **9%** | [Express](#express) | `6586` | `1065` | `8544` |


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
    Reqs/sec      8694.84    4419.64   52685.52
    Latency        5.74ms     4.11ms   362.20ms
    HTTP codes:
      1xx - 0, 2xx - 93048, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6952
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6952
    Throughput:     1.84MB/s
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
    Reqs/sec      6600.58    1044.76    8582.19
    Latency        7.57ms     3.54ms   336.28ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.89MB/s
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
    Reqs/sec     23226.59    6180.97   35011.43
    Latency        2.15ms     1.96ms   175.44ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.26MB/s
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
    Reqs/sec     20676.74    5636.60   29608.87
    Latency        2.42ms     1.95ms   178.59ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.66MB/s
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
    Reqs/sec     62693.70    3717.37   66336.60
    Latency      795.52us    76.96us     5.26ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.90MB/s
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
    Reqs/sec     17472.72    5780.79   43071.58
    Latency        2.84ms     2.22ms   193.27ms
    HTTP codes:
      1xx - 0, 2xx - 95417, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4583
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4583
    Throughput:     3.79MB/s
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
    Reqs/sec     25911.00    8013.08   62668.54
    Latency        1.92ms     1.96ms   166.35ms
    HTTP codes:
      1xx - 0, 2xx - 96617, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3383
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3383
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
    Reqs/sec     73300.74    5564.09   83140.76
    Latency      674.32us   158.37us     9.63ms
    HTTP codes:
      1xx - 0, 2xx - 96863, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3137
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3137
    Throughput:    11.23MB/s
  ```


