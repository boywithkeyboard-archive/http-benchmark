## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73033` | `3808` | `91447` |
| **82%** | [Hyper Express](#hyper-express) | `60214` | `3527` | `66818` |
| **33%** | [Node (Default)](#node-default) | `23762` | `7784` | `63043` |
| **30%** | [Fastify](#fastify) | `21894` | `6887` | `35571` |
| **27%** | [Hono](#hono) | `20002` | `5900` | `28480` |
| **24%** | [Koa](#koa) | `17202` | `8054` | `69948` |
| **11%** | [Carbon](#carbon) | `7937` | `1517` | `10290` |
| **8%** | [Express](#express) | `6069` | `1052` | `8197` |


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
    Reqs/sec      8876.33    7698.09   80947.40
    Latency        5.62ms     4.56ms   391.74ms
    HTTP codes:
      1xx - 0, 2xx - 87323, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12677
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12677
    Throughput:     1.76MB/s
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
    Reqs/sec      6056.03    1065.83    8296.65
    Latency        8.25ms     3.80ms   362.31ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.73MB/s
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
    Reqs/sec     22129.26    6906.41   34303.98
    Latency        2.26ms     2.10ms   190.09ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.01MB/s
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
    Reqs/sec     20014.91    5793.14   28465.30
    Latency        2.50ms     2.08ms   191.02ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.52MB/s
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
    Reqs/sec     58629.05    3390.47   66523.59
    Latency        0.85ms    77.24us     3.42ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.32MB/s
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
    Reqs/sec     17786.56    8678.17   72963.31
    Latency        2.81ms     2.38ms   214.28ms
    HTTP codes:
      1xx - 0, 2xx - 91519, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8481
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8481
    Throughput:     3.68MB/s
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
    Reqs/sec     23033.39    6815.14   61755.65
    Latency        2.17ms     1.97ms   169.91ms
    HTTP codes:
      1xx - 0, 2xx - 97333, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2667
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2667
    Throughput:     5.13MB/s
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
    Reqs/sec     72662.00    2708.65   80034.68
    Latency      684.14us   188.05us    10.50ms
    HTTP codes:
      1xx - 0, 2xx - 96413, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3587
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3587
    Throughput:    11.09MB/s
  ```


