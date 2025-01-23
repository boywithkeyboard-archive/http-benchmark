## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74936` | `5687` | `84963` |
| **86%** | [Hyper Express](#hyper-express) | `64591` | `4347` | `71049` |
| **37%** | [Node (Default)](#node-default) | `27476` | `8790` | `63962` |
| **32%** | [Fastify](#fastify) | `23956` | `7509` | `36588` |
| **29%** | [Hono](#hono) | `21429` | `5952` | `30558` |
| **25%** | [Koa](#koa) | `18668` | `6681` | `55520` |
| **11%** | [Carbon](#carbon) | `8410` | `1465` | `10448` |
| **9%** | [Express](#express) | `6522` | `1062` | `8565` |


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
    Reqs/sec      8810.53    4331.45   51057.99
    Latency        5.66ms     4.28ms   371.03ms
    HTTP codes:
      1xx - 0, 2xx - 93824, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6176
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6176
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
    Reqs/sec      6971.23    4826.18   55151.99
    Latency        7.16ms     3.93ms   356.68ms
    HTTP codes:
      1xx - 0, 2xx - 91098, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8902
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8902
    Throughput:     1.82MB/s
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
    Reqs/sec     24087.24    7031.74   37232.20
    Latency        2.07ms     2.01ms   178.35ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.46MB/s
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
    Reqs/sec     21818.88    5845.42   29688.69
    Latency        2.29ms     2.10ms   189.61ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.93MB/s
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
    Reqs/sec     62877.14    3763.78   67323.75
    Latency      792.93us    69.56us     2.77ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.93MB/s
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
    Reqs/sec     18726.70    7303.98   57341.34
    Latency        2.66ms     2.32ms   204.22ms
    HTTP codes:
      1xx - 0, 2xx - 92074, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7926
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7926
    Throughput:     3.90MB/s
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
    Reqs/sec     25704.41    8172.05   52101.26
    Latency        1.94ms     1.95ms   168.33ms
    HTTP codes:
      1xx - 0, 2xx - 97676, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2324
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2324
    Throughput:     5.74MB/s
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
    Reqs/sec     75454.83    6614.01   96028.28
    Latency      660.12us   161.53us     6.96ms
    HTTP codes:
      1xx - 0, 2xx - 98037, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1963
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1963
    Throughput:    11.71MB/s
  ```


