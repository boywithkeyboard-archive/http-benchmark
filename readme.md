## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73865` | `5755` | `84723` |
| **86%** | [Hyper Express](#hyper-express) | `63524` | `4429` | `74723` |
| **36%** | [Node (Default)](#node-default) | `26338` | `7986` | `55795` |
| **33%** | [Fastify](#fastify) | `24207` | `7212` | `35855` |
| **29%** | [Hono](#hono) | `21100` | `5556` | `29509` |
| **25%** | [Koa](#koa) | `18666` | `6691` | `59009` |
| **11%** | [Carbon](#carbon) | `8362` | `1448` | `10591` |
| **9%** | [Express](#express) | `6590` | `1031` | `8647` |


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
    Reqs/sec      8533.84    3564.66   47625.05
    Latency        5.85ms     4.25ms   368.70ms
    HTTP codes:
      1xx - 0, 2xx - 95553, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4447
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4447
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
    Reqs/sec      6548.21    1028.46    8624.37
    Latency        7.63ms     3.45ms   336.22ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.87MB/s
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
    Reqs/sec     24153.58    7469.51   36439.32
    Latency        2.07ms     1.96ms   180.40ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.48MB/s
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
    Reqs/sec     20975.86    5909.83   30165.44
    Latency        2.38ms     2.00ms   180.90ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.74MB/s
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
    Reqs/sec     63329.74    3621.79   65448.95
    Latency      787.23us    61.83us     3.02ms
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
    Reqs/sec     17889.74    5947.03   48272.93
    Latency        2.79ms     2.41ms   208.71ms
    HTTP codes:
      1xx - 0, 2xx - 94706, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5294
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5294
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
    Reqs/sec     27033.76    7702.89   53098.86
    Latency        1.85ms     1.92ms   162.99ms
    HTTP codes:
      1xx - 0, 2xx - 97243, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2757
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2757
    Throughput:     6.02MB/s
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
    Reqs/sec     73356.05    6474.41   83822.47
    Latency      679.99us   200.79us    11.13ms
    HTTP codes:
      1xx - 0, 2xx - 97036, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2964
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2964
    Throughput:    11.25MB/s
  ```


