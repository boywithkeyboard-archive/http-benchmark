## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `101922` | `3750` | `106178` |
| **88%** | [Hyper Express](#hyper-express) | `89405` | `4156` | `104051` |
| **45%** | [Node (Default)](#node-default) | `46020` | `13485` | `88146` |
| **42%** | [Fastify](#fastify) | `42845` | `12979` | `66319` |
| **36%** | [Koa](#koa) | `37158` | `15479` | `105206` |
| **36%** | [Hono](#hono) | `36653` | `9702` | `58795` |
| **11%** | [Carbon](#carbon) | `11500` | `2369` | `17407` |
| **10%** | [Express](#express) | `9732` | `2187` | `13668` |


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
    Reqs/sec     13141.75    8946.85   88847.94
    Latency        3.80ms     3.32ms   285.66ms
    HTTP codes:
      1xx - 0, 2xx - 90620, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9380
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9380
    Throughput:     2.71MB/s
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
    Reqs/sec     10363.27    9144.87   95844.60
    Latency        4.81ms     2.97ms   271.00ms
    HTTP codes:
      1xx - 0, 2xx - 88279, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11721
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11721
    Throughput:     2.62MB/s
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
    Reqs/sec     43111.87   13443.91   65648.30
    Latency        1.16ms     1.60ms   139.52ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.78MB/s
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
    Reqs/sec     39969.30   12148.87   59221.15
    Latency        1.25ms     1.48ms   130.47ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.03MB/s
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
    Reqs/sec     88814.32    3970.81   92264.73
    Latency      560.97us    51.68us     2.37ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    12.63MB/s
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
    Reqs/sec     36533.72   14634.97   99586.17
    Latency        1.36ms     1.82ms   154.21ms
    HTTP codes:
      1xx - 0, 2xx - 91015, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8985
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8985
    Throughput:     7.52MB/s
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
    Reqs/sec     45496.24   14530.40  110231.44
    Latency        1.10ms     1.33ms   114.45ms
    HTTP codes:
      1xx - 0, 2xx - 94957, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5043
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5043
    Throughput:     9.90MB/s
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
    Reqs/sec    102083.22    4419.26  116682.64
    Latency      489.30us   112.69us     4.04ms
    HTTP codes:
      1xx - 0, 2xx - 97026, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2974
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2974
    Throughput:    15.63MB/s
  ```


