## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74292` | `5189` | `88550` |
| **85%** | [Hyper Express](#hyper-express) | `63511` | `4831` | `83889` |
| **33%** | [Node (Default)](#node-default) | `24425` | `7395` | `69973` |
| **30%** | [Fastify](#fastify) | `22649` | `6553` | `37522` |
| **30%** | [Hono](#hono) | `22609` | `6422` | `31353` |
| **25%** | [Koa](#koa) | `18366` | `8396` | `68108` |
| **11%** | [Carbon](#carbon) | `7913` | `1321` | `10792` |
| **9%** | [Express](#express) | `6467` | `1106` | `8432` |


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
    Reqs/sec      8716.66    6838.94   73253.31
    Latency        5.73ms     4.28ms   369.20ms
    HTTP codes:
      1xx - 0, 2xx - 89676, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10324
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10324
    Throughput:     1.78MB/s
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
    Reqs/sec      6415.55    1130.65    8416.00
    Latency        7.79ms     3.71ms   358.91ms
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
    Reqs/sec     22841.14    6115.09   37800.11
    Latency        2.19ms     1.95ms   177.15ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.18MB/s
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
    Reqs/sec     23560.54    6721.28   31057.85
    Latency        2.12ms     1.93ms   173.30ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.32MB/s
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
    Reqs/sec     63045.69    3703.32   66818.01
    Latency      790.42us    78.48us     3.22ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.96MB/s
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
    Reqs/sec     18545.00    8724.95   69896.84
    Latency        2.69ms     2.46ms   212.01ms
    HTTP codes:
      1xx - 0, 2xx - 92215, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7785
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7785
    Throughput:     3.87MB/s
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
    Reqs/sec     22656.10    6614.54   66875.83
    Latency        2.20ms     1.84ms   160.27ms
    HTTP codes:
      1xx - 0, 2xx - 96871, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3129
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3129
    Throughput:     5.02MB/s
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
    Reqs/sec     75213.09    4672.59   95164.91
    Latency      662.67us   163.04us     7.09ms
    HTTP codes:
      1xx - 0, 2xx - 96820, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3180
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3180
    Throughput:    11.51MB/s
  ```


