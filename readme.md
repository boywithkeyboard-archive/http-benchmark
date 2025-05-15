## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73364` | `4745` | `83167` |
| **85%** | [Hyper Express](#hyper-express) | `62290` | `3694` | `65432` |
| **36%** | [Node (Default)](#node-default) | `26087` | `7464` | `52138` |
| **33%** | [Fastify](#fastify) | `24482` | `7291` | `37377` |
| **29%** | [Hono](#hono) | `21271` | `5997` | `30608` |
| **26%** | [Koa](#koa) | `18747` | `7510` | `55529` |
| **11%** | [Carbon](#carbon) | `8225` | `1410` | `10286` |
| **9%** | [Express](#express) | `6508` | `1117` | `8551` |


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
    Reqs/sec      8596.14    4548.65   53021.18
    Latency        5.81ms     4.38ms   374.56ms
    HTTP codes:
      1xx - 0, 2xx - 92977, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7023
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7023
    Throughput:     1.81MB/s
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
    Reqs/sec      6362.08     977.21    8445.79
    Latency        7.86ms     3.65ms   348.92ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.82MB/s
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
    Reqs/sec     24135.99    7595.01   37072.58
    Latency        2.07ms     2.07ms   184.51ms
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
    Reqs/sec     20335.94    6065.32   30272.42
    Latency        2.46ms     2.14ms   191.28ms
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
    Reqs/sec     61456.33    3376.23   64532.70
    Latency      811.43us    75.25us     3.38ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.73MB/s
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
    Reqs/sec     19060.17    6892.15   58359.55
    Latency        2.62ms     2.42ms   215.38ms
    HTTP codes:
      1xx - 0, 2xx - 94194, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5806
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5806
    Throughput:     4.06MB/s
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
    Reqs/sec     27216.17    8647.90   57563.88
    Latency        1.83ms     1.97ms   167.79ms
    HTTP codes:
      1xx - 0, 2xx - 96903, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3097
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3097
    Throughput:     6.05MB/s
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
    Reqs/sec     72819.69    5091.53   82271.51
    Latency      684.20us   182.42us    15.20ms
    HTTP codes:
      1xx - 0, 2xx - 98088, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1912
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1912
    Throughput:    11.29MB/s
  ```


