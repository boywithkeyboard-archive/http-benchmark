## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74798` | `1408` | `80249` |
| **84%** | [Hyper Express](#hyper-express) | `62621` | `3913` | `67595` |
| **35%** | [Node (Default)](#node-default) | `26006` | `8082` | `62417` |
| **31%** | [Fastify](#fastify) | `23475` | `7018` | `35663` |
| **28%** | [Hono](#hono) | `20581` | `5354` | `29216` |
| **25%** | [Koa](#koa) | `18499` | `7383` | `67610` |
| **11%** | [Carbon](#carbon) | `8001` | `1431` | `10242` |
| **8%** | [Express](#express) | `6319` | `1045` | `8281` |


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
    Reqs/sec      8967.54    7274.65   75078.55
    Latency        5.56ms     4.29ms   368.69ms
    HTTP codes:
      1xx - 0, 2xx - 88606, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11394
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11394
    Throughput:     1.81MB/s
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
    Reqs/sec      6920.33    5499.97   68543.35
    Latency        7.21ms     4.17ms   380.21ms
    HTTP codes:
      1xx - 0, 2xx - 90570, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9430
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9430
    Throughput:     1.79MB/s
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
    Reqs/sec     22972.86    6819.95   34541.18
    Latency        2.17ms     2.04ms   183.44ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.22MB/s
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
    Reqs/sec     20885.23    5993.70   29426.55
    Latency        2.39ms     2.08ms   186.49ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.72MB/s
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
    Reqs/sec     62189.44    3695.14   65151.22
    Latency      801.93us    66.57us     2.58ms
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
    Reqs/sec     17828.07    7569.21   65156.03
    Latency        2.80ms     2.38ms   208.73ms
    HTTP codes:
      1xx - 0, 2xx - 93075, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6925
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6925
    Throughput:     3.75MB/s
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
    Reqs/sec     24733.89    8040.30   78682.81
    Latency        2.02ms     1.98ms   171.47ms
    HTTP codes:
      1xx - 0, 2xx - 96244, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3756
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3756
    Throughput:     5.45MB/s
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
    Reqs/sec     73796.28    2764.08   76914.89
    Latency      675.01us   136.96us     5.79ms
    HTTP codes:
      1xx - 0, 2xx - 97558, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2442
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2442
    Throughput:    11.39MB/s
  ```


