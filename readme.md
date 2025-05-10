## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72676` | `4265` | `90276` |
| **85%** | [Hyper Express](#hyper-express) | `61529` | `3320` | `68370` |
| **35%** | [Node (Default)](#node-default) | `25271` | `7292` | `44782` |
| **33%** | [Fastify](#fastify) | `24228` | `7271` | `35729` |
| **28%** | [Hono](#hono) | `20351` | `5752` | `30167` |
| **26%** | [Koa](#koa) | `18613` | `6471` | `50106` |
| **11%** | [Carbon](#carbon) | `8239` | `1387` | `10239` |
| **9%** | [Express](#express) | `6487` | `1014` | `8392` |


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
    Reqs/sec      8619.04    3659.29   55138.60
    Latency        5.79ms     4.30ms   375.01ms
    HTTP codes:
      1xx - 0, 2xx - 95021, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4979
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4979
    Throughput:     1.86MB/s
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
    Reqs/sec      6426.63    1027.94    8255.31
    Latency        7.78ms     3.67ms   353.39ms
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
    Reqs/sec     23880.08    6985.57   35516.99
    Latency        2.09ms     1.87ms   172.22ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.42MB/s
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
    Reqs/sec     20350.21    5913.32   29349.82
    Latency        2.45ms     2.08ms   186.12ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.60MB/s
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
    Reqs/sec     60810.67    3511.58   64638.77
    Latency      820.29us    71.81us     2.95ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.64MB/s
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
    Reqs/sec     19631.58    6775.83   51332.30
    Latency        2.54ms     2.36ms   207.16ms
    HTTP codes:
      1xx - 0, 2xx - 94803, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5197
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5197
    Throughput:     4.21MB/s
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
    Reqs/sec     25988.42    7450.66   49064.60
    Latency        1.92ms     1.80ms   158.15ms
    HTTP codes:
      1xx - 0, 2xx - 97911, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2089
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2089
    Throughput:     5.83MB/s
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
    Reqs/sec     72819.19    4651.48   83098.24
    Latency      684.17us   161.47us     7.52ms
    HTTP codes:
      1xx - 0, 2xx - 97702, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2298
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2298
    Throughput:    11.25MB/s
  ```


