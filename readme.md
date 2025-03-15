## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73778` | `5200` | `82167` |
| **86%** | [Hyper Express](#hyper-express) | `63163` | `4213` | `73150` |
| **35%** | [Node (Default)](#node-default) | `25699` | `7109` | `56115` |
| **34%** | [Fastify](#fastify) | `25002` | `7921` | `37342` |
| **29%** | [Hono](#hono) | `21035` | `5861` | `29740` |
| **26%** | [Koa](#koa) | `19015` | `7377` | `61772` |
| **11%** | [Carbon](#carbon) | `8335` | `1372` | `10448` |
| **9%** | [Express](#express) | `6596` | `1099` | `8519` |


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
    Reqs/sec      8881.31    5172.60   64292.13
    Latency        5.62ms     4.21ms   368.55ms
    HTTP codes:
      1xx - 0, 2xx - 92461, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7539
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7539
    Throughput:     1.87MB/s
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
    Reqs/sec      7026.97    4276.08   51919.58
    Latency        7.10ms     3.93ms   362.45ms
    HTTP codes:
      1xx - 0, 2xx - 92195, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7805
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7805
    Throughput:     1.86MB/s
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
    Reqs/sec     24704.88    7520.21   36579.28
    Latency        2.02ms     2.00ms   178.28ms
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
    Reqs/sec     21812.24    5875.90   29915.51
    Latency        2.29ms     2.02ms   182.31ms
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
    Reqs/sec     63075.39    3450.97   68834.46
    Latency      789.96us    75.52us     3.27ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.97MB/s
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
    Reqs/sec     20680.06    8099.98   59214.54
    Latency        2.41ms     2.30ms   200.02ms
    HTTP codes:
      1xx - 0, 2xx - 92238, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7762
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7762
    Throughput:     4.31MB/s
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
    Reqs/sec     25834.65    6846.82   48655.78
    Latency        1.93ms     1.88ms   161.53ms
    HTTP codes:
      1xx - 0, 2xx - 97853, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2147
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2147
    Throughput:     5.79MB/s
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
    Reqs/sec     73997.79    5102.98   81453.26
    Latency      673.24us   186.42us     8.48ms
    HTTP codes:
      1xx - 0, 2xx - 97380, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2620
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2620
    Throughput:    11.40MB/s
  ```


