## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75972` | `5690` | `85662` |
| **83%** | [Hyper Express](#hyper-express) | `63425` | `3504` | `67243` |
| **34%** | [Node (Default)](#node-default) | `26209` | `7582` | `55983` |
| **33%** | [Fastify](#fastify) | `25128` | `7837` | `36477` |
| **28%** | [Hono](#hono) | `21308` | `5678` | `30193` |
| **25%** | [Koa](#koa) | `18885` | `6523` | `52724` |
| **11%** | [Carbon](#carbon) | `8262` | `1414` | `10467` |
| **9%** | [Express](#express) | `6508` | `1055` | `8485` |


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
    Reqs/sec      9115.75    4620.88   53707.02
    Latency        5.48ms     4.25ms   370.52ms
    HTTP codes:
      1xx - 0, 2xx - 93424, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6576
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6576
    Throughput:     1.93MB/s
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
    Reqs/sec      6966.24    4582.04   54912.84
    Latency        7.17ms     3.86ms   350.09ms
    HTTP codes:
      1xx - 0, 2xx - 92061, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7939
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7939
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
    Reqs/sec     25097.99    7660.96   36415.82
    Latency        1.99ms     2.01ms   178.12ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.70MB/s
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
    Reqs/sec     21696.19    6262.52   30610.62
    Latency        2.30ms     2.04ms   181.49ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.90MB/s
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
    Reqs/sec     64687.47    4299.81   70782.04
    Latency      769.62us    77.33us     3.12ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.20MB/s
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
    Reqs/sec     19370.86    7355.74   57186.53
    Latency        2.58ms     2.35ms   208.70ms
    HTTP codes:
      1xx - 0, 2xx - 92205, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7795
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7795
    Throughput:     4.04MB/s
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
    Reqs/sec     26037.55    7810.04   55637.52
    Latency        1.92ms     2.03ms   169.64ms
    HTTP codes:
      1xx - 0, 2xx - 96855, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3145
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3145
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
    Reqs/sec     74733.48    4971.29   81398.73
    Latency      667.01us   177.12us     8.21ms
    HTTP codes:
      1xx - 0, 2xx - 97363, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2637
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2637
    Throughput:    11.51MB/s
  ```


