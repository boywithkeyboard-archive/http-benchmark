## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75407` | `2198` | `85313` |
| **83%** | [Hyper Express](#hyper-express) | `62819` | `3749` | `66413` |
| **31%** | [Node (Default)](#node-default) | `23174` | `5868` | `60823` |
| **30%** | [Fastify](#fastify) | `22864` | `6143` | `36250` |
| **26%** | [Hono](#hono) | `19312` | `4386` | `29010` |
| **23%** | [Koa](#koa) | `17507` | `10335` | `82790` |
| **11%** | [Carbon](#carbon) | `8276` | `1397` | `10520` |
| **9%** | [Express](#express) | `6549` | `1101` | `8515` |


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
    Reqs/sec      8891.73    5270.19   68616.81
    Latency        5.61ms     4.26ms   369.56ms
    HTTP codes:
      1xx - 0, 2xx - 93268, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6732
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6732
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
    Reqs/sec      6404.32    1022.54    8523.32
    Latency        7.80ms     3.64ms   350.88ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.83MB/s
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
    Reqs/sec     22307.16    5763.18   36603.32
    Latency        2.24ms     1.97ms   178.50ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.06MB/s
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
    Reqs/sec     18648.70    4428.53   30167.11
    Latency        2.68ms     2.00ms   177.81ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.21MB/s
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
    Reqs/sec     63723.40    3777.68   66634.42
    Latency      782.69us    71.52us     2.76ms
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
    Reqs/sec     16835.01    7695.68   78949.64
    Latency        2.97ms     2.45ms   215.02ms
    HTTP codes:
      1xx - 0, 2xx - 92220, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7780
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7780
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
    Reqs/sec     23663.66    7824.39   78974.48
    Latency        2.11ms     1.79ms   153.74ms
    HTTP codes:
      1xx - 0, 2xx - 95684, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4316
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4316
    Throughput:     5.18MB/s
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
    Reqs/sec     76015.96    2695.13   84395.16
    Latency      655.17us   150.81us     8.51ms
    HTTP codes:
      1xx - 0, 2xx - 96711, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3289
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3289
    Throughput:    11.63MB/s
  ```


