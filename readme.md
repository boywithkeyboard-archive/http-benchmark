## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `71520` | `2563` | `79968` |
| **82%** | [Hyper Express](#hyper-express) | `58956` | `2951` | `61938` |
| **33%** | [Node (Default)](#node-default) | `23721` | `7903` | `62475` |
| **31%** | [Fastify](#fastify) | `21948` | `6881` | `34595` |
| **27%** | [Hono](#hono) | `18971` | `5424` | `28408` |
| **26%** | [Koa](#koa) | `18257` | `9218` | `76768` |
| **11%** | [Carbon](#carbon) | `7715` | `1448` | `10109` |
| **9%** | [Express](#express) | `6263` | `1115` | `8322` |


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
    Reqs/sec      8947.18    7467.32   72173.30
    Latency        5.57ms     4.47ms   386.83ms
    HTTP codes:
      1xx - 0, 2xx - 88051, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11949
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11949
    Throughput:     1.79MB/s
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
    Reqs/sec      6267.44    1121.59    8258.28
    Latency        7.98ms     3.83ms   367.53ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     23510.85    7898.09   35931.70
    Latency        2.12ms     2.16ms   194.49ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.34MB/s
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
    Reqs/sec     19388.25    5509.50   29249.48
    Latency        2.58ms     2.18ms   195.57ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.38MB/s
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
    Reqs/sec     58919.38    3116.33   61244.31
    Latency      846.37us    70.39us     3.16ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.37MB/s
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
    Reqs/sec     18179.35    8725.47   74531.61
    Latency        2.74ms     2.44ms   213.64ms
    HTTP codes:
      1xx - 0, 2xx - 92190, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7810
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7810
    Throughput:     3.79MB/s
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
    Reqs/sec     23638.03    7715.84   58336.78
    Latency        2.11ms     2.03ms   175.64ms
    HTTP codes:
      1xx - 0, 2xx - 97626, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2374
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2374
    Throughput:     5.29MB/s
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
    Reqs/sec     72581.62    4205.70   85795.56
    Latency      688.06us   196.77us    13.53ms
    HTTP codes:
      1xx - 0, 2xx - 97397, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2603
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2603
    Throughput:    11.16MB/s
  ```


