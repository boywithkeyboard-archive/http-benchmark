## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73442` | `5921` | `83051` |
| **85%** | [Hyper Express](#hyper-express) | `62635` | `4291` | `71860` |
| **35%** | [Node (Default)](#node-default) | `25747` | `7706` | `46914` |
| **34%** | [Fastify](#fastify) | `24841` | `7674` | `34930` |
| **29%** | [Hono](#hono) | `21293` | `5952` | `29388` |
| **25%** | [Koa](#koa) | `18357` | `5921` | `45242` |
| **11%** | [Carbon](#carbon) | `8129` | `1322` | `10421` |
| **9%** | [Express](#express) | `6334` | `973` | `8276` |


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
    Reqs/sec      8634.44    3438.89   51093.47
    Latency        5.78ms     4.24ms   369.93ms
    HTTP codes:
      1xx - 0, 2xx - 95797, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4203
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4203
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
    Reqs/sec      6446.81    1048.65    8274.62
    Latency        7.75ms     3.67ms   351.16ms
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
    Reqs/sec     24274.93    7226.74   35102.15
    Latency        2.06ms     1.99ms   176.66ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.50MB/s
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
    Reqs/sec     20047.35    5516.86   28683.95
    Latency        2.49ms     1.94ms   173.82ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.53MB/s
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
    Reqs/sec     62052.41    3891.72   71166.35
    Latency      803.95us    69.85us     3.21ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.81MB/s
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
    Reqs/sec     16916.02    5860.88   49029.61
    Latency        2.94ms     2.37ms   208.04ms
    HTTP codes:
      1xx - 0, 2xx - 94204, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5796
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5796
    Throughput:     3.61MB/s
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
    Reqs/sec     24920.14    7206.81   63734.82
    Latency        2.00ms     1.89ms   163.22ms
    HTTP codes:
      1xx - 0, 2xx - 97253, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2747
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2747
    Throughput:     5.55MB/s
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
    Reqs/sec     73211.78    5789.59   81224.20
    Latency      680.54us   198.59us    12.66ms
    HTTP codes:
      1xx - 0, 2xx - 96759, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3241
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3241
    Throughput:    11.21MB/s
  ```


