## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73665` | `4868` | `81177` |
| **85%** | [Hyper Express](#hyper-express) | `62311` | `3429` | `65781` |
| **36%** | [Node (Default)](#node-default) | `26407` | `7853` | `56786` |
| **35%** | [Fastify](#fastify) | `25475` | `8067` | `36605` |
| **28%** | [Hono](#hono) | `20599` | `5590` | `30999` |
| **25%** | [Koa](#koa) | `18463` | `6797` | `53087` |
| **11%** | [Carbon](#carbon) | `7979` | `1311` | `10212` |
| **9%** | [Express](#express) | `6492` | `1069` | `8483` |


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
    Reqs/sec      9060.86    5362.41   57771.95
    Latency        5.51ms     4.27ms   368.30ms
    HTTP codes:
      1xx - 0, 2xx - 91393, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8607
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8607
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
    Reqs/sec      6883.59    3800.38   49808.44
    Latency        7.25ms     3.95ms   362.42ms
    HTTP codes:
      1xx - 0, 2xx - 93843, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6157
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6157
    Throughput:     1.85MB/s
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
    Reqs/sec     24963.52    8141.46   37924.59
    Latency        2.00ms     2.05ms   186.03ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.67MB/s
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
    Reqs/sec     20749.59    5835.33   30253.53
    Latency        2.41ms     1.96ms   174.99ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.68MB/s
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
    Reqs/sec     60998.58    4757.33   64235.21
    Latency      817.69us    90.50us     3.41ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.66MB/s
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
    Reqs/sec     19196.05    7914.30   60490.70
    Latency        2.60ms     2.34ms   206.13ms
    HTTP codes:
      1xx - 0, 2xx - 91419, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8581
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8581
    Throughput:     3.97MB/s
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
    Reqs/sec     26784.03    8508.57   56427.82
    Latency        1.86ms     1.90ms   164.06ms
    HTTP codes:
      1xx - 0, 2xx - 97562, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2438
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2438
    Throughput:     5.98MB/s
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
    Reqs/sec     74168.81    5005.72   83498.67
    Latency      672.08us   174.36us     8.12ms
    HTTP codes:
      1xx - 0, 2xx - 97958, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2042
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2042
    Throughput:    11.50MB/s
  ```


