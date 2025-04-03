## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73198` | `6010` | `83074` |
| **86%** | [Hyper Express](#hyper-express) | `62980` | `4126` | `70432` |
| **37%** | [Node (Default)](#node-default) | `27314` | `8668` | `63005` |
| **32%** | [Fastify](#fastify) | `23629` | `7137` | `37189` |
| **29%** | [Hono](#hono) | `21057` | `6095` | `30096` |
| **26%** | [Koa](#koa) | `18807` | `7281` | `59125` |
| **11%** | [Carbon](#carbon) | `8207` | `1381` | `10243` |
| **9%** | [Express](#express) | `6426` | `1026` | `8398` |


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
    Reqs/sec      8822.81    4233.68   48423.73
    Latency        5.65ms     4.43ms   378.52ms
    HTTP codes:
      1xx - 0, 2xx - 92400, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7600
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7600
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
    Reqs/sec      6837.61    4000.69   50792.83
    Latency        7.30ms     3.90ms   357.96ms
    HTTP codes:
      1xx - 0, 2xx - 93061, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6939
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6939
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
    Reqs/sec     23065.80    6596.33   36160.77
    Latency        2.17ms     2.09ms   184.66ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.23MB/s
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
    Reqs/sec     21068.15    6177.23   30166.57
    Latency        2.37ms     1.98ms   179.79ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.76MB/s
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
    Reqs/sec     63456.51    3616.00   67057.91
    Latency      786.07us    67.96us     3.45ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.01MB/s
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
    Reqs/sec     18511.77    6060.31   51458.44
    Latency        2.70ms     2.36ms   208.09ms
    HTTP codes:
      1xx - 0, 2xx - 95346, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4654
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4654
    Throughput:     3.99MB/s
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
    Reqs/sec     24532.39    6698.17   44819.30
    Latency        2.03ms     1.83ms   156.35ms
    HTTP codes:
      1xx - 0, 2xx - 98220, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1780
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1780
    Throughput:     5.52MB/s
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
    Reqs/sec     75687.72    5613.57   91994.40
    Latency      659.27us   182.99us    10.63ms
    HTTP codes:
      1xx - 0, 2xx - 97945, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2055
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2055
    Throughput:    11.72MB/s
  ```


