## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75860` | `4500` | `91318` |
| **85%** | [Hyper Express](#hyper-express) | `64152` | `3962` | `70591` |
| **33%** | [Node (Default)](#node-default) | `25245` | `7588` | `62085` |
| **31%** | [Fastify](#fastify) | `23584` | `7117` | `34625` |
| **28%** | [Hono](#hono) | `21166` | `6196` | `30294` |
| **25%** | [Koa](#koa) | `18645` | `7801` | `66297` |
| **11%** | [Carbon](#carbon) | `8013` | `1384` | `10359` |
| **9%** | [Express](#express) | `6538` | `1118` | `9674` |


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
    Reqs/sec      9184.94    6753.72   76417.22
    Latency        5.43ms     4.25ms   370.73ms
    HTTP codes:
      1xx - 0, 2xx - 89936, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10064
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10064
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
    Reqs/sec      6856.02    5758.43   66867.63
    Latency        7.29ms     4.03ms   368.19ms
    HTTP codes:
      1xx - 0, 2xx - 90321, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9679
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9679
    Throughput:     1.77MB/s
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
    Reqs/sec     24252.07    7446.43   34101.49
    Latency        2.06ms     1.96ms   172.82ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.50MB/s
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
    Reqs/sec     20639.41    6099.31   30057.26
    Latency        2.42ms     2.06ms   185.81ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.66MB/s
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
    Reqs/sec     63781.88    4061.95   69216.63
    Latency      782.05us    87.16us     2.96ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.06MB/s
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
    Reqs/sec     19440.98    8051.24   66803.65
    Latency        2.56ms     2.39ms   210.62ms
    HTTP codes:
      1xx - 0, 2xx - 93335, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6665
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6665
    Throughput:     4.11MB/s
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
    Reqs/sec     26575.74    8511.24   57395.26
    Latency        1.87ms     1.86ms   161.83ms
    HTTP codes:
      1xx - 0, 2xx - 97566, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2434
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2434
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
    Reqs/sec     74746.14    2467.05   81174.66
    Latency      666.60us   139.17us     5.60ms
    HTTP codes:
      1xx - 0, 2xx - 96826, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3174
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3174
    Throughput:    11.45MB/s
  ```


