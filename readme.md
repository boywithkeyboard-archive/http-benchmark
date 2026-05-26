## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `80967` | `2715` | `87137` |
| **86%** | [Hyper Express](#hyper-express) | `69810` | `3569` | `72765` |
| **43%** | [Node (Default)](#node-default) | `35172` | `9849` | `69198` |
| **43%** | [Fastify](#fastify) | `34519` | `11413` | `52161` |
| **39%** | [Koa](#koa) | `31618` | `13186` | `83292` |
| **37%** | [Hono](#hono) | `29802` | `9157` | `45507` |
| **12%** | [Carbon](#carbon) | `9366` | `2320` | `13485` |
| **10%** | [Express](#express) | `7715` | `1803` | `10637` |


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
    Reqs/sec     10596.51    7911.66   92484.53
    Latency        4.71ms     4.31ms   369.51ms
    HTTP codes:
      1xx - 0, 2xx - 90169, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9831
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9831
    Throughput:     2.17MB/s
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
    Reqs/sec      8340.65    7299.97   82196.99
    Latency        5.99ms     3.75ms   340.73ms
    HTTP codes:
      1xx - 0, 2xx - 89001, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10999
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10999
    Throughput:     2.12MB/s
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
    Reqs/sec     32310.72    9683.76   52476.16
    Latency        1.55ms     1.90ms   170.95ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.33MB/s
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
    Reqs/sec     31096.10    9866.05   47324.56
    Latency        1.61ms     1.96ms   174.42ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.01MB/s
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
    Reqs/sec     70300.34    3437.16   77418.15
    Latency      709.97us    62.78us     2.76ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.98MB/s
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
    Reqs/sec     28385.27   12124.79   76620.26
    Latency        1.76ms     2.31ms   198.80ms
    HTTP codes:
      1xx - 0, 2xx - 92578, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7422
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7422
    Throughput:     5.93MB/s
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
    Reqs/sec     33333.58    9161.55   79224.12
    Latency        1.50ms     1.68ms   147.58ms
    HTTP codes:
      1xx - 0, 2xx - 96480, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3520
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3520
    Throughput:     7.35MB/s
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
    Reqs/sec     80315.56    3744.08   86190.68
    Latency      618.19us   147.63us     7.21ms
    HTTP codes:
      1xx - 0, 2xx - 96777, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3223
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3223
    Throughput:    12.34MB/s
  ```


