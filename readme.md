## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73672` | `5602` | `78599` |
| **85%** | [Hyper Express](#hyper-express) | `62500` | `3942` | `71607` |
| **36%** | [Node (Default)](#node-default) | `26205` | `7415` | `40690` |
| **32%** | [Fastify](#fastify) | `23835` | `7233` | `35618` |
| **28%** | [Hono](#hono) | `20478` | `5559` | `29375` |
| **25%** | [Koa](#koa) | `18523` | `5911` | `54257` |
| **11%** | [Carbon](#carbon) | `8301` | `1417` | `10531` |
| **9%** | [Express](#express) | `6497` | `995` | `8539` |


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
    Reqs/sec      8832.96    3989.85   51177.06
    Latency        5.65ms     4.16ms   360.73ms
    HTTP codes:
      1xx - 0, 2xx - 94187, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5813
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5813
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
    Reqs/sec      7453.35    5609.07   56096.33
    Latency        6.69ms     3.78ms   344.66ms
    HTTP codes:
      1xx - 0, 2xx - 88318, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11682
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11660
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 22
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
    Reqs/sec     24208.42    7329.84   36126.36
    Latency        2.06ms     1.98ms   176.69ms
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
    Reqs/sec     20390.78    5254.98   28590.11
    Latency        2.45ms     1.96ms   176.17ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.61MB/s
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
    Reqs/sec     62983.87    3708.10   68848.97
    Latency      791.75us    69.78us     2.85ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.95MB/s
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
    Reqs/sec     17920.62    6172.62   47951.91
    Latency        2.78ms     2.30ms   200.84ms
    HTTP codes:
      1xx - 0, 2xx - 94589, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5411
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5411
    Throughput:     3.83MB/s
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
    Reqs/sec     24621.37    6097.75   46912.65
    Latency        2.02ms     1.80ms   155.66ms
    HTTP codes:
      1xx - 0, 2xx - 97819, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2181
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2181
    Throughput:     5.53MB/s
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
    Reqs/sec     75310.74    6664.26   96829.48
    Latency      663.11us   195.37us    10.23ms
    HTTP codes:
      1xx - 0, 2xx - 97833, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2167
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2167
    Throughput:    11.63MB/s
  ```


