## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72429` | `4888` | `79215` |
| **85%** | [Hyper Express](#hyper-express) | `61752` | `3902` | `68430` |
| **37%** | [Node (Default)](#node-default) | `26472` | `8351` | `53627` |
| **32%** | [Fastify](#fastify) | `23394` | `6837` | `35019` |
| **27%** | [Hono](#hono) | `19890` | `5570` | `29365` |
| **25%** | [Koa](#koa) | `18431` | `7242` | `61624` |
| **11%** | [Carbon](#carbon) | `8188` | `1426` | `10196` |
| **9%** | [Express](#express) | `6269` | `988` | `8343` |


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
    Reqs/sec      8633.94    4991.65   58472.31
    Latency        5.78ms     4.34ms   377.57ms
    HTTP codes:
      1xx - 0, 2xx - 92099, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7901
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7901
    Throughput:     1.81MB/s
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
    Reqs/sec      6368.07    1045.53    8356.00
    Latency        7.85ms     3.69ms   354.70ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     24760.06    7898.11   36245.67
    Latency        2.02ms     1.91ms   174.91ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.61MB/s
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
    Reqs/sec     19892.61    5857.92   28922.29
    Latency        2.51ms     2.07ms   187.02ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.49MB/s
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
    Reqs/sec     61216.20    3526.76   64787.28
    Latency      814.37us    81.18us     3.02ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.70MB/s
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
    Reqs/sec     18659.62    6776.98   55531.51
    Latency        2.67ms     2.36ms   206.38ms
    HTTP codes:
      1xx - 0, 2xx - 93582, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6418
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6418
    Throughput:     3.95MB/s
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
    Reqs/sec     24583.60    7059.52   53036.24
    Latency        2.03ms     1.90ms   164.34ms
    HTTP codes:
      1xx - 0, 2xx - 96977, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3023
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3023
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
    Reqs/sec     72075.36    5317.35   81968.63
    Latency      691.53us   185.91us     6.74ms
    HTTP codes:
      1xx - 0, 2xx - 97573, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2427
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2427
    Throughput:    11.13MB/s
  ```


