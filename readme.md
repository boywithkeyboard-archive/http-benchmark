## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `134045` | `7731` | `141351` |
| **85%** | [Hyper Express](#hyper-express) | `113903` | `7156` | `122453` |
| **36%** | [Node (Default)](#node-default) | `48704` | `13592` | `122964` |
| **33%** | [Fastify](#fastify) | `44539` | `9251` | `69400` |
| **29%** | [Hono](#hono) | `38547` | `8551` | `57653` |
| **28%** | [Koa](#koa) | `37158` | `17313` | `135395` |
| **12%** | [Carbon](#carbon) | `16544` | `4334` | `23782` |
| **9%** | [Express](#express) | `11685` | `2323` | `16303` |


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
    Reqs/sec     18370.33   12610.08  121960.16
    Latency        2.71ms     3.81ms   311.18ms
    HTTP codes:
      1xx - 0, 2xx - 89129, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10871
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10871
    Throughput:     3.72MB/s
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
    Reqs/sec     12917.52   13668.44  120856.43
    Latency        3.86ms     3.06ms   273.63ms
    HTTP codes:
      1xx - 0, 2xx - 84259, 3xx - 0, 4xx - 0, 5xx - 0
      others - 15741
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 15741
    Throughput:     3.12MB/s
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
    Reqs/sec     50349.66   21293.87  133179.34
    Latency        0.99ms     1.31ms   105.36ms
    HTTP codes:
      1xx - 0, 2xx - 76740, 3xx - 0, 4xx - 0, 5xx - 0
      others - 23260
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 23260
    Throughput:     8.78MB/s
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
    Reqs/sec     41508.13   16938.07  120982.02
    Latency        1.20ms     1.38ms   105.29ms
    HTTP codes:
      1xx - 0, 2xx - 88582, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11418
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11418
    Throughput:     8.33MB/s
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
    Reqs/sec    113386.14    7232.22  121024.73
    Latency      438.43us   225.59us     9.90ms
    HTTP codes:
      1xx - 0, 2xx - 91596, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8404
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8404
    Throughput:    14.74MB/s
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
    Reqs/sec     37167.46   14693.64  111206.16
    Latency        1.34ms     1.40ms   115.75ms
    HTTP codes:
      1xx - 0, 2xx - 89783, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10217
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10217
    Throughput:     7.56MB/s
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
    Reqs/sec     47742.24   11459.96  109386.91
    Latency        1.04ms     1.14ms    92.72ms
    HTTP codes:
      1xx - 0, 2xx - 96043, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3957
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3957
    Throughput:    10.50MB/s
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
    Reqs/sec    134654.70    6244.17  142785.54
    Latency      367.94us   126.77us     5.37ms
    HTTP codes:
      1xx - 0, 2xx - 94368, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5632
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5632
    Throughput:    20.15MB/s
  ```


