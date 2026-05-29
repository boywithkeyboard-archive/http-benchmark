## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `105247` | `4179` | `117231` |
| **85%** | [Hyper Express](#hyper-express) | `89847` | `4218` | `92508` |
| **48%** | [Node (Default)](#node-default) | `50969` | `16042` | `94533` |
| **41%** | [Fastify](#fastify) | `43576` | `12369` | `66430` |
| **39%** | [Hono](#hono) | `41486` | `12143` | `61597` |
| **37%** | [Koa](#koa) | `38933` | `19484` | `113274` |
| **11%** | [Carbon](#carbon) | `11395` | `2197` | `17249` |
| **9%** | [Express](#express) | `9451` | `1935` | `13725` |


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
    Reqs/sec     13601.67   10667.63  105768.92
    Latency        3.67ms     3.29ms   283.37ms
    HTTP codes:
      1xx - 0, 2xx - 88484, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11516
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11516
    Throughput:     2.73MB/s
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
    Reqs/sec     10590.27   10173.92  105671.07
    Latency        4.71ms     2.97ms   274.54ms
    HTTP codes:
      1xx - 0, 2xx - 87079, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12921
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12911
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 10
    Throughput:     2.64MB/s
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
    Reqs/sec     45761.63   17190.60   97128.76
    Latency        1.09ms     1.54ms   131.64ms
    HTTP codes:
      1xx - 0, 2xx - 83190, 3xx - 0, 4xx - 0, 5xx - 0
      others - 16810
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 16809
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 1
    Throughput:     8.64MB/s
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
    Reqs/sec     40624.99   13982.83   97024.17
    Latency        1.23ms     1.66ms   139.87ms
    HTTP codes:
      1xx - 0, 2xx - 92311, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7689
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7689
    Throughput:     8.48MB/s
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
    Reqs/sec     87264.80    9231.94  101724.05
    Latency      571.33us   317.05us    12.22ms
    HTTP codes:
      1xx - 0, 2xx - 88783, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11217
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11217
    Throughput:    10.99MB/s
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
    Reqs/sec     37754.24   15562.19   99785.86
    Latency        1.32ms     1.87ms   160.55ms
    HTTP codes:
      1xx - 0, 2xx - 91204, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8796
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8796
    Throughput:     7.78MB/s
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
    Reqs/sec     52096.74   16905.13  103459.68
    Latency        0.96ms     1.37ms   115.96ms
    HTTP codes:
      1xx - 0, 2xx - 95037, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4963
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4963
    Throughput:    11.34MB/s
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
    Reqs/sec    104516.62    4118.06  114632.96
    Latency      475.87us   129.54us     6.69ms
    HTTP codes:
      1xx - 0, 2xx - 96579, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3421
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3421
    Throughput:    15.98MB/s
  ```


