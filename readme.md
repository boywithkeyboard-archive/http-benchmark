## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `71790` | `3269` | `84521` |
| **83%** | [Hyper Express](#hyper-express) | `59493` | `3512` | `66101` |
| **34%** | [Node (Default)](#node-default) | `24273` | `8118` | `71972` |
| **31%** | [Fastify](#fastify) | `22259` | `6489` | `36134` |
| **30%** | [Hono](#hono) | `21196` | `6661` | `30283` |
| **26%** | [Koa](#koa) | `18666` | `9398` | `72624` |
| **11%** | [Carbon](#carbon) | `7986` | `1400` | `10370` |
| **9%** | [Express](#express) | `6213` | `1050` | `8316` |


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
    Reqs/sec      8808.66    6503.84   73444.03
    Latency        5.67ms     4.36ms   372.77ms
    HTTP codes:
      1xx - 0, 2xx - 89899, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10101
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10101
    Throughput:     1.80MB/s
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
    Reqs/sec      6200.34    1034.42    8355.76
    Latency        8.06ms     3.73ms   358.76ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.77MB/s
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
    Reqs/sec     22816.97    6548.57   35884.50
    Latency        2.19ms     2.16ms   192.75ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.18MB/s
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
    Reqs/sec     20495.51    6315.30   31029.00
    Latency        2.44ms     2.10ms   191.60ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.63MB/s
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
    Reqs/sec     60503.60    4003.57   68438.90
    Latency      824.33us    89.38us     3.18ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.59MB/s
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
    Reqs/sec     17657.62    8420.99   70718.25
    Latency        2.82ms     2.44ms   218.00ms
    HTTP codes:
      1xx - 0, 2xx - 91384, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8616
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8616
    Throughput:     3.65MB/s
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
    Reqs/sec     24070.50    6939.37   69578.58
    Latency        2.07ms     2.10ms   179.50ms
    HTTP codes:
      1xx - 0, 2xx - 96550, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3450
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3450
    Throughput:     5.32MB/s
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
    Reqs/sec     72715.78    4521.77   83867.80
    Latency      684.22us   214.20us    15.14ms
    HTTP codes:
      1xx - 0, 2xx - 95987, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4013
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4013
    Throughput:    11.05MB/s
  ```


