## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74195` | `5633` | `87692` |
| **86%** | [Hyper Express](#hyper-express) | `63546` | `5779` | `78101` |
| **36%** | [Node (Default)](#node-default) | `26556` | `7087` | `53911` |
| **34%** | [Fastify](#fastify) | `25441` | `7962` | `37581` |
| **30%** | [Hono](#hono) | `22053` | `6510` | `30556` |
| **26%** | [Koa](#koa) | `19023` | `6750` | `53626` |
| **11%** | [Carbon](#carbon) | `8271` | `1429` | `10474` |
| **9%** | [Express](#express) | `6589` | `1085` | `8532` |


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
    Reqs/sec      9211.72    5346.91   60174.53
    Latency        5.42ms     4.17ms   359.95ms
    HTTP codes:
      1xx - 0, 2xx - 91981, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8019
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8019
    Throughput:     1.93MB/s
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
    Reqs/sec      7172.55    5450.77   60751.94
    Latency        6.96ms     3.92ms   355.25ms
    HTTP codes:
      1xx - 0, 2xx - 89616, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10384
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10384
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
    Reqs/sec     25439.65    7628.10   36313.33
    Latency        1.96ms     1.97ms   178.25ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.77MB/s
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
    Reqs/sec     22251.89    6120.54   30932.97
    Latency        2.25ms     1.99ms   177.71ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.03MB/s
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
    Reqs/sec     63168.02    3790.70   66918.32
    Latency      789.17us    78.45us     4.08ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.98MB/s
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
    Reqs/sec     19003.46    6913.54   59155.06
    Latency        2.63ms     2.28ms   200.41ms
    HTTP codes:
      1xx - 0, 2xx - 93883, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6117
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6117
    Throughput:     4.04MB/s
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
    Reqs/sec     26595.78    7538.59   50315.62
    Latency        1.88ms     2.07ms   171.83ms
    HTTP codes:
      1xx - 0, 2xx - 97743, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2257
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2257
    Throughput:     5.95MB/s
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
    Reqs/sec     74818.91    5070.21   83463.53
    Latency      665.39us   211.01us    13.38ms
    HTTP codes:
      1xx - 0, 2xx - 95902, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4098
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4098
    Throughput:    11.35MB/s
  ```


