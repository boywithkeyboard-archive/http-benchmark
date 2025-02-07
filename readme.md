## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72510` | `4300` | `76624` |
| **88%** | [Hyper Express](#hyper-express) | `63861` | `3815` | `69302` |
| **37%** | [Node (Default)](#node-default) | `26514` | `7881` | `52741` |
| **34%** | [Fastify](#fastify) | `24730` | `6979` | `36348` |
| **29%** | [Hono](#hono) | `21342` | `6179` | `30189` |
| **26%** | [Koa](#koa) | `19005` | `7263` | `54112` |
| **11%** | [Carbon](#carbon) | `8294` | `1447` | `10422` |
| **9%** | [Express](#express) | `6527` | `1061` | `8494` |


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
    Reqs/sec      8839.23    4312.95   54638.99
    Latency        5.65ms     4.31ms   370.84ms
    HTTP codes:
      1xx - 0, 2xx - 94139, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5861
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5861
    Throughput:     1.89MB/s
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
    Reqs/sec      6887.77    4024.45   53063.84
    Latency        7.25ms     3.88ms   353.02ms
    HTTP codes:
      1xx - 0, 2xx - 93003, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6997
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6997
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
    Reqs/sec     25457.30    7706.28   36546.05
    Latency        1.96ms     2.03ms   182.59ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.77MB/s
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
    Reqs/sec     21571.98    5828.63   30793.36
    Latency        2.32ms     1.98ms   175.18ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.87MB/s
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
    Reqs/sec     62667.00    3015.67   66792.50
    Latency      795.70us    69.12us     3.43ms
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
    Reqs/sec     18660.52    6875.07   55857.83
    Latency        2.67ms     2.34ms   204.87ms
    HTTP codes:
      1xx - 0, 2xx - 93796, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6204
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6204
    Throughput:     3.96MB/s
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
    Reqs/sec     26560.61    7618.29   56838.44
    Latency        1.88ms     1.86ms   157.26ms
    HTTP codes:
      1xx - 0, 2xx - 97475, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2525
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2525
    Throughput:     5.92MB/s
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
    Reqs/sec     72765.13    4902.91   85616.35
    Latency      684.47us   183.41us     9.28ms
    HTTP codes:
      1xx - 0, 2xx - 96804, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3196
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3196
    Throughput:    11.15MB/s
  ```


