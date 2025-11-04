## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `76673` | `3239` | `87598` |
| **82%** | [Hyper Express](#hyper-express) | `63025` | `3755` | `66247` |
| **36%** | [Node (Default)](#node-default) | `27246` | `8653` | `71624` |
| **31%** | [Fastify](#fastify) | `23930` | `7636` | `37048` |
| **28%** | [Hono](#hono) | `21736` | `6208` | `31698` |
| **25%** | [Koa](#koa) | `18872` | `9240` | `82474` |
| **11%** | [Carbon](#carbon) | `8113` | `1353` | `10460` |
| **8%** | [Express](#express) | `6424` | `1079` | `8699` |


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
    Reqs/sec      9045.92    5906.93   78762.68
    Latency        5.52ms     4.28ms   367.29ms
    HTTP codes:
      1xx - 0, 2xx - 92162, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7838
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7838
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
    Reqs/sec      6954.36    5957.15   75110.25
    Latency        7.17ms     4.00ms   365.93ms
    HTTP codes:
      1xx - 0, 2xx - 90039, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9961
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9961
    Throughput:     1.79MB/s
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
    Reqs/sec     24666.18    7548.28   36334.51
    Latency        2.03ms     2.01ms   181.59ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.59MB/s
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
    Reqs/sec     20281.45    5245.42   31051.94
    Latency        2.46ms     1.97ms   175.40ms
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
    Reqs/sec     63696.87    3504.92   66839.19
    Latency      782.82us    65.02us     3.66ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.05MB/s
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
    Reqs/sec     18693.40    8968.38   81983.81
    Latency        2.67ms     2.32ms   203.87ms
    HTTP codes:
      1xx - 0, 2xx - 91398, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8602
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8602
    Throughput:     3.86MB/s
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
    Reqs/sec     26898.05    8763.22   62101.59
    Latency        1.86ms     1.89ms   163.19ms
    HTTP codes:
      1xx - 0, 2xx - 97444, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2556
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2556
    Throughput:     6.00MB/s
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
    Reqs/sec     76580.28    3293.90   81104.93
    Latency      650.97us   128.64us     5.69ms
    HTTP codes:
      1xx - 0, 2xx - 97461, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2539
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2539
    Throughput:    11.81MB/s
  ```


