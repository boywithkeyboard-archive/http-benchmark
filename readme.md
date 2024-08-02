## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73016` | `8509` | `86823` |
| **87%** | [Hyper Express](#hyper-express) | `63429` | `3741` | `68418` |
| **36%** | [Node (Default)](#node-default) | `25982` | `7281` | `47612` |
| **31%** | [Fastify](#fastify) | `22973` | `6522` | `35481` |
| **28%** | [Hono](#hono) | `20374` | `5646` | `29698` |
| **24%** | [Koa](#koa) | `17589` | `6727` | `57745` |
| **11%** | [Carbon](#carbon) | `8064` | `1341` | `10310` |
| **9%** | [Express](#express) | `6451` | `1028` | `8456` |


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
    Reqs/sec      8951.97    5303.85   65143.56
    Latency        5.58ms     4.14ms   362.01ms
    HTTP codes:
      1xx - 0, 2xx - 91733, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8267
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8267
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
    Reqs/sec      6581.52    1086.10    8425.09
    Latency        7.59ms     3.60ms   346.51ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.88MB/s
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
    Reqs/sec     23614.95    6967.95   34985.06
    Latency        2.11ms     1.97ms   177.82ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.36MB/s
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
    Reqs/sec     20332.50    5701.12   29483.09
    Latency        2.46ms     1.99ms   178.23ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.59MB/s
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
    Reqs/sec     63330.99    3612.42   66396.20
    Latency      787.26us    64.45us     3.42ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.99MB/s
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
    Reqs/sec     18103.91    6616.60   52935.04
    Latency        2.75ms     2.37ms   205.50ms
    HTTP codes:
      1xx - 0, 2xx - 94457, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5543
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5543
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
    Reqs/sec     26288.76    7712.33   57840.27
    Latency        1.90ms     1.85ms   158.06ms
    HTTP codes:
      1xx - 0, 2xx - 95515, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4485
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4485
    Throughput:     5.75MB/s
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
    Reqs/sec     74755.07    5625.89   83299.32
    Latency      668.31us   191.27us    12.94ms
    HTTP codes:
      1xx - 0, 2xx - 97686, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2314
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2308
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 6
    Throughput:    11.50MB/s
  ```


