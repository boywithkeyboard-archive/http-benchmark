## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74372` | `3127` | `79031` |
| **84%** | [Hyper Express](#hyper-express) | `62836` | `5398` | `97006` |
| **34%** | [Node (Default)](#node-default) | `25654` | `8796` | `73563` |
| **33%** | [Fastify](#fastify) | `24371` | `7580` | `36751` |
| **28%** | [Hono](#hono) | `21040` | `5861` | `29736` |
| **25%** | [Koa](#koa) | `18616` | `9171` | `79507` |
| **11%** | [Carbon](#carbon) | `8400` | `1495` | `10503` |
| **9%** | [Express](#express) | `6486` | `1086` | `8368` |


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
    Reqs/sec      8876.99    5834.55   76853.68
    Latency        5.62ms     4.26ms   369.35ms
    HTTP codes:
      1xx - 0, 2xx - 91874, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8126
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8126
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
    Reqs/sec      6423.17    1078.71    8433.58
    Latency        7.78ms     3.77ms   359.01ms
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
    Reqs/sec     24398.39    7678.23   37134.91
    Latency        2.05ms     1.97ms   178.11ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.54MB/s
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
    Reqs/sec     20739.12    5664.92   30482.42
    Latency        2.41ms     2.07ms   186.57ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.68MB/s
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
    Reqs/sec     62913.20    3821.48   73120.80
    Latency      792.51us    72.13us     3.19ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.94MB/s
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
    Reqs/sec     18516.06    8124.08   76001.72
    Latency        2.69ms     2.41ms   215.24ms
    HTTP codes:
      1xx - 0, 2xx - 91963, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8037
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8037
    Throughput:     3.85MB/s
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
    Reqs/sec     26596.91    8523.97   67141.74
    Latency        1.88ms     1.83ms   162.05ms
    HTTP codes:
      1xx - 0, 2xx - 96900, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3100
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3100
    Throughput:     5.90MB/s
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
    Reqs/sec     75294.05    3400.86   84344.49
    Latency      661.29us   175.83us     8.16ms
    HTTP codes:
      1xx - 0, 2xx - 96745, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3255
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3255
    Throughput:    11.53MB/s
  ```


