## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73662` | `6135` | `83377` |
| **84%** | [Hyper Express](#hyper-express) | `61911` | `3774` | `70368` |
| **37%** | [Node (Default)](#node-default) | `27033` | `7826` | `53931` |
| **34%** | [Fastify](#fastify) | `24893` | `7512` | `35575` |
| **29%** | [Hono](#hono) | `21257` | `6162` | `29203` |
| **25%** | [Koa](#koa) | `18442` | `6315` | `57011` |
| **11%** | [Carbon](#carbon) | `8325` | `1445` | `10437` |
| **9%** | [Express](#express) | `6444` | `1045` | `8511` |


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
    Reqs/sec      8629.53    3424.58   48752.45
    Latency        5.79ms     4.19ms   367.34ms
    HTTP codes:
      1xx - 0, 2xx - 95500, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4500
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4500
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
    Reqs/sec      6439.35    1045.42    8500.65
    Latency        7.76ms     3.63ms   347.09ms
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
    Reqs/sec     23811.49    6954.89   36026.40
    Latency        2.10ms     2.00ms   177.57ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.40MB/s
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
    Reqs/sec     20791.70    5760.93   29549.75
    Latency        2.40ms     2.03ms   182.52ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.70MB/s
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
    Reqs/sec     61558.55    3613.45   65124.21
    Latency      808.87us    71.86us     3.46ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.75MB/s
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
    Reqs/sec     18542.05    6128.67   44276.20
    Latency        2.68ms     2.36ms   207.77ms
    HTTP codes:
      1xx - 0, 2xx - 95238, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4762
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4762
    Throughput:     4.01MB/s
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
    Reqs/sec     26403.33    7665.73   50750.53
    Latency        1.89ms     1.86ms   155.11ms
    HTTP codes:
      1xx - 0, 2xx - 96729, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3271
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3271
    Throughput:     5.85MB/s
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
    Reqs/sec     74236.89    5735.57   82311.12
    Latency      670.75us   174.93us     7.75ms
    HTTP codes:
      1xx - 0, 2xx - 97978, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2022
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2022
    Throughput:    11.46MB/s
  ```


