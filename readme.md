## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73245` | `4836` | `83069` |
| **85%** | [Hyper Express](#hyper-express) | `61905` | `3398` | `64718` |
| **37%** | [Node (Default)](#node-default) | `26796` | `7549` | `55660` |
| **33%** | [Fastify](#fastify) | `24262` | `7198` | `36330` |
| **29%** | [Hono](#hono) | `20886` | `5378` | `29656` |
| **28%** | [Koa](#koa) | `20198` | `7887` | `62634` |
| **11%** | [Carbon](#carbon) | `8269` | `1411` | `10456` |
| **9%** | [Express](#express) | `6606` | `1126` | `8610` |


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
    Reqs/sec      8689.97    3601.31   46758.79
    Latency        5.75ms     4.25ms   367.89ms
    HTTP codes:
      1xx - 0, 2xx - 95128, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4872
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4872
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
    Reqs/sec      7019.88    4367.80   55872.11
    Latency        7.11ms     3.92ms   355.19ms
    HTTP codes:
      1xx - 0, 2xx - 92268, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7732
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7732
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
    Reqs/sec     24811.21    7435.22   36268.82
    Latency        2.01ms     1.99ms   178.35ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.63MB/s
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
    Reqs/sec     21209.37    6054.60   29942.65
    Latency        2.36ms     1.95ms   174.50ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.79MB/s
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
    Reqs/sec     62453.77    3148.52   65268.43
    Latency      798.20us    61.24us     2.83ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.87MB/s
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
    Reqs/sec     19781.80    7360.82   60772.89
    Latency        2.52ms     2.33ms   204.26ms
    HTTP codes:
      1xx - 0, 2xx - 93464, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6536
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6536
    Throughput:     4.19MB/s
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
    Reqs/sec     28160.58    8230.42   46471.72
    Latency        1.77ms     1.89ms   161.10ms
    HTTP codes:
      1xx - 0, 2xx - 98096, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1904
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1904
    Throughput:     6.32MB/s
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
    Reqs/sec     74341.65    5269.19   84102.28
    Latency      670.16us   188.67us     8.17ms
    HTTP codes:
      1xx - 0, 2xx - 96738, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3262
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3262
    Throughput:    11.38MB/s
  ```


