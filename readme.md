## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `101139` | `4124` | `116990` |
| **86%** | [Hyper Express](#hyper-express) | `86601` | `4492` | `89940` |
| **50%** | [Node (Default)](#node-default) | `50377` | `19142` | `119599` |
| **47%** | [Fastify](#fastify) | `47613` | `16256` | `66729` |
| **37%** | [Hono](#hono) | `37299` | `13492` | `57581` |
| **33%** | [Koa](#koa) | `33229` | `13141` | `90161` |
| **12%** | [Carbon](#carbon) | `11686` | `2552` | `17080` |
| **9%** | [Express](#express) | `9334` | `1807` | `13425` |


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
    Reqs/sec     12742.98   10507.47  112164.42
    Latency        3.91ms     3.35ms   285.06ms
    HTTP codes:
      1xx - 0, 2xx - 88275, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11725
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11725
    Throughput:     2.56MB/s
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
    Reqs/sec     10139.84    7926.15   92500.28
    Latency        4.92ms     2.90ms   265.74ms
    HTTP codes:
      1xx - 0, 2xx - 90906, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9094
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9094
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
    Reqs/sec     47772.38   16050.19   65445.57
    Latency        1.05ms     1.45ms   129.74ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    10.84MB/s
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
    Reqs/sec     37475.98   13400.27   59335.10
    Latency        1.33ms     1.65ms   144.91ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.45MB/s
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
    Reqs/sec     88367.92    3867.73   91693.09
    Latency      564.19us    49.63us     2.00ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    12.55MB/s
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
    Reqs/sec     34484.78   12218.98   85691.47
    Latency        1.45ms     1.86ms   162.26ms
    HTTP codes:
      1xx - 0, 2xx - 93225, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6775
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6775
    Throughput:     7.25MB/s
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
    Reqs/sec     50052.34   14972.98   92349.73
    Latency        1.00ms     1.19ms   104.50ms
    HTTP codes:
      1xx - 0, 2xx - 97074, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2926
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2926
    Throughput:    11.12MB/s
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
    Reqs/sec    101222.97    3166.32  107844.31
    Latency      491.68us   176.20us     8.05ms
    HTTP codes:
      1xx - 0, 2xx - 96857, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3143
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3143
    Throughput:    15.52MB/s
  ```


