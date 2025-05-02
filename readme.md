## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72682` | `4257` | `79561` |
| **84%** | [Hyper Express](#hyper-express) | `61191` | `3085` | `63647` |
| **36%** | [Node (Default)](#node-default) | `25922` | `7184` | `47751` |
| **33%** | [Fastify](#fastify) | `24343` | `7459` | `36577` |
| **29%** | [Hono](#hono) | `20735` | `5577` | `29721` |
| **26%** | [Koa](#koa) | `18749` | `6644` | `54398` |
| **11%** | [Carbon](#carbon) | `8080` | `1399` | `10180` |
| **9%** | [Express](#express) | `6461` | `1039` | `8353` |


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
    Reqs/sec      8673.74    4587.05   56158.39
    Latency        5.75ms     4.33ms   376.52ms
    HTTP codes:
      1xx - 0, 2xx - 92955, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7045
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7045
    Throughput:     1.83MB/s
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
    Reqs/sec      6315.36     937.07    8387.07
    Latency        7.91ms     3.67ms   351.91ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.81MB/s
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
    Reqs/sec     25256.96    7724.95   35635.70
    Latency        1.98ms     2.05ms   183.26ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.73MB/s
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
    Reqs/sec     20178.09    5059.77   29391.03
    Latency        2.48ms     2.02ms   179.56ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.56MB/s
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
    Reqs/sec     59922.03    3304.10   68556.17
    Latency      832.42us    78.83us     3.56ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.51MB/s
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
    Reqs/sec     18546.86    6755.01   53718.63
    Latency        2.69ms     2.36ms   206.25ms
    HTTP codes:
      1xx - 0, 2xx - 94349, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5651
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5651
    Throughput:     3.95MB/s
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
    Reqs/sec     26751.00    8511.10   59552.85
    Latency        1.87ms     1.93ms   165.32ms
    HTTP codes:
      1xx - 0, 2xx - 96652, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3348
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3348
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
    Reqs/sec     74535.66    5638.32   83986.51
    Latency      669.03us   167.83us     8.89ms
    HTTP codes:
      1xx - 0, 2xx - 97010, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2990
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2990
    Throughput:    11.44MB/s
  ```


