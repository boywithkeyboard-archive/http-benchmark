## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74823` | `5659` | `79419` |
| **84%** | [Hyper Express](#hyper-express) | `62856` | `4359` | `79196` |
| **37%** | [Node (Default)](#node-default) | `27316` | `9025` | `59413` |
| **33%** | [Fastify](#fastify) | `24430` | `7257` | `36081` |
| **28%** | [Hono](#hono) | `21287` | `5795` | `30091` |
| **24%** | [Koa](#koa) | `18265` | `6405` | `49005` |
| **11%** | [Carbon](#carbon) | `8266` | `1404` | `10451` |
| **9%** | [Express](#express) | `6668` | `1101` | `8643` |


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
    Reqs/sec      8677.40    3180.55   49598.60
    Latency        5.75ms     4.17ms   360.59ms
    HTTP codes:
      1xx - 0, 2xx - 96090, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3910
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3910
    Throughput:     1.89MB/s
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
    Reqs/sec      6780.05    1112.90    8654.21
    Latency        7.37ms     3.45ms   332.70ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.94MB/s
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
    Reqs/sec     24345.93    7250.67   36691.42
    Latency        2.05ms     1.91ms   173.45ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.53MB/s
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
    Reqs/sec     20362.51    5342.08   29353.54
    Latency        2.45ms     1.94ms   176.59ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.60MB/s
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
    Reqs/sec     62256.99    3627.27   66889.17
    Latency      800.99us    66.37us     2.99ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.84MB/s
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
    Reqs/sec     18520.12    5580.95   39299.49
    Latency        2.69ms     2.29ms   202.39ms
    HTTP codes:
      1xx - 0, 2xx - 96137, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3863
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3863
    Throughput:     4.03MB/s
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
    Reqs/sec     26661.38    7879.78   54160.39
    Latency        1.86ms     1.88ms   159.05ms
    HTTP codes:
      1xx - 0, 2xx - 97167, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2833
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2833
    Throughput:     5.96MB/s
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
    Reqs/sec     74781.38    5790.95   82748.40
    Latency      666.96us   181.66us     9.74ms
    HTTP codes:
      1xx - 0, 2xx - 98121, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1879
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1879
    Throughput:    11.60MB/s
  ```


