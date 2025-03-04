## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73900` | `5230` | `83802` |
| **85%** | [Hyper Express](#hyper-express) | `62829` | `3982` | `66801` |
| **37%** | [Node (Default)](#node-default) | `27530` | `8625` | `52199` |
| **32%** | [Fastify](#fastify) | `24003` | `7403` | `36576` |
| **27%** | [Hono](#hono) | `20253` | `5261` | `29066` |
| **25%** | [Koa](#koa) | `18431` | `6878` | `60300` |
| **11%** | [Carbon](#carbon) | `8290` | `1428` | `10372` |
| **9%** | [Express](#express) | `6600` | `1062` | `8396` |


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
    Reqs/sec      8775.63    3768.29   49827.75
    Latency        5.69ms     4.21ms   364.47ms
    HTTP codes:
      1xx - 0, 2xx - 95264, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4736
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4736
    Throughput:     1.90MB/s
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
    Reqs/sec      6960.98    4403.87   59086.22
    Latency        7.17ms     3.84ms   349.56ms
    HTTP codes:
      1xx - 0, 2xx - 92402, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7598
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7598
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
    Reqs/sec     26441.57    8340.24   36889.11
    Latency        1.89ms     1.97ms   179.84ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     6.00MB/s
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
    Reqs/sec     21462.99    5747.92   30647.16
    Latency        2.33ms     2.02ms   182.78ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.85MB/s
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
    Reqs/sec     62088.84    3981.52   65635.18
    Latency      802.45us    96.19us     4.87ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.83MB/s
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
    Reqs/sec     19274.08    6848.43   53390.24
    Latency        2.59ms     2.31ms   201.21ms
    HTTP codes:
      1xx - 0, 2xx - 94285, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5715
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5715
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
    Reqs/sec     26613.51    7534.76   56384.03
    Latency        1.88ms     1.90ms   162.59ms
    HTTP codes:
      1xx - 0, 2xx - 97057, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2943
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2943
    Throughput:     5.91MB/s
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
    Reqs/sec     74981.44    7243.51  105872.96
    Latency      668.12us   180.31us     8.00ms
    HTTP codes:
      1xx - 0, 2xx - 96821, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3179
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3179
    Throughput:    11.42MB/s
  ```


