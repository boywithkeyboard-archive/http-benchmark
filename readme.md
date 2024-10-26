## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74793` | `5510` | `85583` |
| **83%** | [Hyper Express](#hyper-express) | `62074` | `4240` | `71703` |
| **35%** | [Node (Default)](#node-default) | `26037` | `7358` | `55138` |
| **32%** | [Fastify](#fastify) | `24235` | `7339` | `34950` |
| **28%** | [Hono](#hono) | `21030` | `5806` | `29422` |
| **24%** | [Koa](#koa) | `18183` | `6339` | `48083` |
| **11%** | [Carbon](#carbon) | `8219` | `1361` | `10529` |
| **9%** | [Express](#express) | `6637` | `1039` | `8659` |


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
    Reqs/sec      8743.29    4560.44   70122.08
    Latency        5.71ms     4.19ms   360.22ms
    HTTP codes:
      1xx - 0, 2xx - 93231, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6769
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6769
    Throughput:     1.85MB/s
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
    Reqs/sec      6603.77    1101.01    8627.59
    Latency        7.57ms     3.54ms   337.12ms
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
    Reqs/sec     24611.67    7610.16   36002.80
    Latency        2.03ms     2.03ms   183.52ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.58MB/s
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
    Reqs/sec     19995.01    4601.79   28912.43
    Latency        2.50ms     1.93ms   177.38ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.51MB/s
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
    Reqs/sec     62367.88    3661.15   70354.65
    Latency      800.41us    64.26us     3.00ms
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
    Reqs/sec     18920.05    7631.88   55130.59
    Latency        2.64ms     2.29ms   198.52ms
    HTTP codes:
      1xx - 0, 2xx - 90591, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9409
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9409
    Throughput:     3.88MB/s
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
    Reqs/sec     25829.39    7640.13   48109.75
    Latency        1.93ms     1.88ms   163.14ms
    HTTP codes:
      1xx - 0, 2xx - 97507, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2493
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2493
    Throughput:     5.77MB/s
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
    Reqs/sec     74875.44    6427.97   93708.14
    Latency      665.16us   185.13us     9.54ms
    HTTP codes:
      1xx - 0, 2xx - 97712, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2288
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2288
    Throughput:    11.58MB/s
  ```


