## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72045` | `4145` | `82380` |
| **78%** | [Hyper Express](#hyper-express) | `55952` | `5378` | `66969` |
| **30%** | [Hono](#hono) | `21646` | `6629` | `32978` |
| **28%** | [Node (Default)](#node-default) | `20425` | `5211` | `65474` |
| **28%** | [Fastify](#fastify) | `20051` | `5069` | `37415` |
| **25%** | [Koa](#koa) | `17946` | `9176` | `75298` |
| **11%** | [Carbon](#carbon) | `7667` | `1354` | `10512` |
| **9%** | [Express](#express) | `6199` | `1081` | `8271` |


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
    Reqs/sec      7898.97    6293.75   75265.45
    Latency        6.32ms     4.37ms   375.67ms
    HTTP codes:
      1xx - 0, 2xx - 90049, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9951
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9951
    Throughput:     1.62MB/s
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
    Reqs/sec      6234.22    1127.69    8697.09
    Latency        8.02ms     3.79ms   366.12ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.78MB/s
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
    Reqs/sec     19809.03    5186.14   35402.67
    Latency        2.52ms     2.14ms   194.22ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.50MB/s
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
    Reqs/sec     21026.62    6380.76   30594.10
    Latency        2.38ms     2.12ms   192.42ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.75MB/s
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
    Reqs/sec     59069.78    3290.48   65997.24
    Latency      844.30us    84.65us     3.30ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.39MB/s
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
    Reqs/sec     16541.68    7180.95   58715.72
    Latency        3.01ms     2.48ms   215.66ms
    HTTP codes:
      1xx - 0, 2xx - 93659, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6341
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6341
    Throughput:     3.51MB/s
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
    Reqs/sec     20575.22    5078.32   61272.65
    Latency        2.42ms     1.90ms   165.51ms
    HTTP codes:
      1xx - 0, 2xx - 97458, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2542
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2542
    Throughput:     4.59MB/s
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
    Reqs/sec     75480.78    4448.61   90153.95
    Latency      659.75us   158.08us     6.26ms
    HTTP codes:
      1xx - 0, 2xx - 97262, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2738
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2738
    Throughput:    11.62MB/s
  ```


