## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73794` | `5643` | `81074` |
| **86%** | [Hyper Express](#hyper-express) | `63764` | `3943` | `72693` |
| **35%** | [Node (Default)](#node-default) | `25967` | `7752` | `42777` |
| **33%** | [Fastify](#fastify) | `24219` | `7004` | `35775` |
| **30%** | [Hono](#hono) | `22006` | `6439` | `31214` |
| **25%** | [Koa](#koa) | `18453` | `6034` | `49832` |
| **11%** | [Carbon](#carbon) | `8082` | `1424` | `10254` |
| **8%** | [Express](#express) | `6241` | `1012` | `8499` |


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
    Reqs/sec      8649.40    3528.37   49754.30
    Latency        5.77ms     4.19ms   365.06ms
    HTTP codes:
      1xx - 0, 2xx - 95580, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4420
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4420
    Throughput:     1.88MB/s
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
    Reqs/sec      6443.62     959.74    8477.49
    Latency        7.76ms     3.45ms   331.04ms
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
    Reqs/sec     24088.98    7418.02   36587.19
    Latency        2.07ms     2.05ms   185.12ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.47MB/s
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
    Reqs/sec     21147.37    5980.74   30209.00
    Latency        2.36ms     1.99ms   178.90ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.77MB/s
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
    Reqs/sec     63332.12    3740.78   66149.78
    Latency      787.16us    74.95us     6.16ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.00MB/s
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
    Reqs/sec     17698.96    7048.97   58815.66
    Latency        2.82ms     2.30ms   198.26ms
    HTTP codes:
      1xx - 0, 2xx - 92126, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7874
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7874
    Throughput:     3.69MB/s
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
    Reqs/sec     25664.63    7912.94   61863.41
    Latency        1.94ms     1.91ms   164.05ms
    HTTP codes:
      1xx - 0, 2xx - 96674, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3326
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3326
    Throughput:     5.69MB/s
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
    Reqs/sec     74499.67    5807.02   84075.36
    Latency      669.59us   135.56us     7.30ms
    HTTP codes:
      1xx - 0, 2xx - 98092, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1908
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1908
    Throughput:    11.55MB/s
  ```


