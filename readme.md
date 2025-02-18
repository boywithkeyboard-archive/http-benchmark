## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74838` | `5420` | `90738` |
| **85%** | [Hyper Express](#hyper-express) | `63614` | `4292` | `69696` |
| **35%** | [Node (Default)](#node-default) | `26492` | `7957` | `50277` |
| **33%** | [Fastify](#fastify) | `24410` | `7150` | `37483` |
| **28%** | [Hono](#hono) | `20856` | `5518` | `30129` |
| **25%** | [Koa](#koa) | `19052` | `6807` | `52290` |
| **11%** | [Carbon](#carbon) | `8169` | `1392` | `10461` |
| **8%** | [Express](#express) | `6301` | `1001` | `8431` |


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
    Reqs/sec      8553.01    3903.89   53965.71
    Latency        5.84ms     4.26ms   368.80ms
    HTTP codes:
      1xx - 0, 2xx - 94755, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5245
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5245
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
    Reqs/sec      6434.02    1014.73    8478.08
    Latency        7.77ms     3.62ms   345.47ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     24844.19    7587.22   36434.36
    Latency        2.01ms     1.97ms   179.60ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.64MB/s
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
    Reqs/sec     21474.36    5942.45   30332.26
    Latency        2.33ms     2.02ms   181.99ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.85MB/s
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
    Reqs/sec     62263.98    3952.03   70525.36
    Latency      800.01us    76.72us     3.35ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.85MB/s
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
    Reqs/sec     18230.82    6501.82   55633.07
    Latency        2.73ms     2.43ms   210.34ms
    HTTP codes:
      1xx - 0, 2xx - 93639, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6361
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6361
    Throughput:     3.87MB/s
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
    Reqs/sec     25785.03    7589.75   62851.73
    Latency        1.94ms     1.99ms   168.03ms
    HTTP codes:
      1xx - 0, 2xx - 97146, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2854
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2854
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
    Reqs/sec     75039.75    5619.90   79339.78
    Latency      663.65us   162.11us     6.92ms
    HTTP codes:
      1xx - 0, 2xx - 97103, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2897
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2897
    Throughput:    11.53MB/s
  ```


