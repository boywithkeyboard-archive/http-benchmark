## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72108` | `3370` | `79917` |
| **85%** | [Hyper Express](#hyper-express) | `61286` | `3689` | `71358` |
| **37%** | [Node (Default)](#node-default) | `26535` | `7246` | `59866` |
| **32%** | [Fastify](#fastify) | `22795` | `6531` | `36097` |
| **28%** | [Hono](#hono) | `20418` | `5324` | `29890` |
| **27%** | [Koa](#koa) | `19177` | `8247` | `64586` |
| **11%** | [Carbon](#carbon) | `8028` | `1349` | `10143` |
| **9%** | [Express](#express) | `6276` | `1006` | `8304` |


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
    Reqs/sec      8497.30    4508.68   54544.18
    Latency        5.88ms     4.40ms   383.02ms
    HTTP codes:
      1xx - 0, 2xx - 93156, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6844
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6844
    Throughput:     1.80MB/s
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
    Reqs/sec      6428.35    1071.01    8369.60
    Latency        7.77ms     3.68ms   354.20ms
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
    Reqs/sec     24184.29    7639.59   35924.22
    Latency        2.06ms     2.08ms   185.54ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.49MB/s
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
    Reqs/sec     19957.15    5570.11   29525.45
    Latency        2.50ms     2.00ms   178.05ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.51MB/s
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
    Reqs/sec     61674.88    4013.29   69288.48
    Latency      809.67us    80.25us     3.17ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.75MB/s
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
    Reqs/sec     18861.60    6907.97   51980.79
    Latency        2.64ms     2.39ms   210.35ms
    HTTP codes:
      1xx - 0, 2xx - 93958, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6042
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6042
    Throughput:     4.01MB/s
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
    Reqs/sec     24476.49    6783.78   56113.95
    Latency        2.04ms     2.02ms   173.14ms
    HTTP codes:
      1xx - 0, 2xx - 97343, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2657
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2657
    Throughput:     5.46MB/s
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
    Reqs/sec     73109.44    3928.29   79479.93
    Latency      680.93us   179.42us     8.37ms
    HTTP codes:
      1xx - 0, 2xx - 96656, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3344
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3344
    Throughput:    11.18MB/s
  ```


