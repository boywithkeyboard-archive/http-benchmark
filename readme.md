## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `69965` | `2967` | `77404` |
| **82%** | [Hyper Express](#hyper-express) | `57432` | `3834` | `62925` |
| **31%** | [Node (Default)](#node-default) | `21345` | `5524` | `60008` |
| **30%** | [Hono](#hono) | `21166` | `6744` | `31723` |
| **29%** | [Fastify](#fastify) | `20371` | `4657` | `36802` |
| **26%** | [Koa](#koa) | `18053` | `8340` | `66380` |
| **11%** | [Carbon](#carbon) | `7634` | `1368` | `10287` |
| **9%** | [Express](#express) | `6090` | `1099` | `8383` |


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
    Reqs/sec      8850.63    7278.49   74963.38
    Latency        5.64ms     4.53ms   391.66ms
    HTTP codes:
      1xx - 0, 2xx - 87919, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12081
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12081
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
    Reqs/sec      5943.90    1040.87    8940.96
    Latency        8.41ms     3.78ms   358.09ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.70MB/s
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
    Reqs/sec     21767.98    6655.41   36415.32
    Latency        2.29ms     2.06ms   182.22ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.94MB/s
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
    Reqs/sec     20212.12    6402.46   30476.77
    Latency        2.47ms     2.23ms   201.86ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.56MB/s
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
    Reqs/sec     57985.95    3869.59   65314.38
    Latency        0.86ms    91.77us     4.79ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.25MB/s
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
    Reqs/sec     17797.64    8197.29   66222.18
    Latency        2.80ms     2.58ms   223.94ms
    HTTP codes:
      1xx - 0, 2xx - 92688, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7312
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7312
    Throughput:     3.73MB/s
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
    Reqs/sec     21586.67    5627.69   57274.77
    Latency        2.31ms     2.12ms   183.00ms
    HTTP codes:
      1xx - 0, 2xx - 97604, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2396
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2396
    Throughput:     4.83MB/s
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
    Reqs/sec     69975.49    3082.01   79009.59
    Latency      711.06us   190.77us    10.30ms
    HTTP codes:
      1xx - 0, 2xx - 96630, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3370
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3370
    Throughput:    10.71MB/s
  ```


