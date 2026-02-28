## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `71894` | `3484` | `83594` |
| **84%** | [Hyper Express](#hyper-express) | `60600` | `4899` | `92237` |
| **33%** | [Node (Default)](#node-default) | `23925` | `6478` | `59401` |
| **31%** | [Fastify](#fastify) | `21953` | `6647` | `36767` |
| **29%** | [Hono](#hono) | `20934` | `5979` | `30458` |
| **25%** | [Koa](#koa) | `18106` | `8095` | `69362` |
| **11%** | [Carbon](#carbon) | `8087` | `1456` | `10302` |
| **9%** | [Express](#express) | `6204` | `987` | `8366` |


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
    Reqs/sec      9032.59    6586.04   73800.21
    Latency        5.53ms     4.30ms   372.53ms
    HTTP codes:
      1xx - 0, 2xx - 90018, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9982
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9982
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
    Reqs/sec      6431.14    1112.45    8289.35
    Latency        7.77ms     3.79ms   366.23ms
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
    Reqs/sec     22860.59    6423.31   36391.86
    Latency        2.19ms     2.00ms   180.30ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.19MB/s
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
    Reqs/sec     21000.73    6346.20   30026.38
    Latency        2.38ms     2.12ms   188.45ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.75MB/s
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
    Reqs/sec     59190.11    3202.80   65832.88
    Latency      842.72us    76.16us     3.10ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.41MB/s
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
    Reqs/sec     18775.12    8081.97   64015.52
    Latency        2.66ms     2.45ms   213.90ms
    HTTP codes:
      1xx - 0, 2xx - 92912, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7088
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7088
    Throughput:     3.94MB/s
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
    Reqs/sec     24823.92    7111.64   73747.70
    Latency        2.01ms     1.93ms   165.54ms
    HTTP codes:
      1xx - 0, 2xx - 96265, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3735
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3735
    Throughput:     5.47MB/s
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
    Reqs/sec     72426.95    4082.04   82512.17
    Latency      686.52us   176.99us    10.62ms
    HTTP codes:
      1xx - 0, 2xx - 96622, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3378
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3378
    Throughput:    11.07MB/s
  ```


