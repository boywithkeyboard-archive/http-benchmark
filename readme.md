## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73203` | `4723` | `81151` |
| **83%** | [Hyper Express](#hyper-express) | `60878` | `3984` | `64810` |
| **34%** | [Node (Default)](#node-default) | `24695` | `7298` | `45761` |
| **33%** | [Fastify](#fastify) | `24247` | `7420` | `36097` |
| **29%** | [Hono](#hono) | `21079` | `5835` | `29990` |
| **25%** | [Koa](#koa) | `18517` | `7232` | `54337` |
| **11%** | [Carbon](#carbon) | `8400` | `1480` | `10550` |
| **9%** | [Express](#express) | `6404` | `1055` | `8515` |


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
    Reqs/sec      8363.00    4466.45   52253.78
    Latency        5.97ms     4.29ms   373.05ms
    HTTP codes:
      1xx - 0, 2xx - 92991, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7009
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7009
    Throughput:     1.77MB/s
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
    Reqs/sec      6448.21    1063.30    8585.78
    Latency        7.75ms     3.56ms   347.34ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.85MB/s
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
    Reqs/sec     24323.05    7902.14   36788.09
    Latency        2.05ms     2.12ms   189.88ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.52MB/s
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
    Reqs/sec     20991.74    6087.56   30113.22
    Latency        2.38ms     2.01ms   181.28ms
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
    Reqs/sec     61022.78    3466.78   69713.78
    Latency      817.27us    73.53us     2.73ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.67MB/s
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
    Reqs/sec     18938.72    6883.10   50565.10
    Latency        2.64ms     2.50ms   220.56ms
    HTTP codes:
      1xx - 0, 2xx - 94315, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5685
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5685
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
    Reqs/sec     26807.20    8358.92   50515.07
    Latency        1.86ms     1.86ms   159.72ms
    HTTP codes:
      1xx - 0, 2xx - 97880, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2120
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2120
    Throughput:     6.01MB/s
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
    Reqs/sec     73131.67    4826.04   82970.78
    Latency      681.16us   204.45us    11.66ms
    HTTP codes:
      1xx - 0, 2xx - 96692, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3308
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3308
    Throughput:    11.19MB/s
  ```


