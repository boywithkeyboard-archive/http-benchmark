## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `100492` | `7783` | `113228` |
| **89%** | [Hyper Express](#hyper-express) | `89471` | `4182` | `95005` |
| **46%** | [Fastify](#fastify) | `46139` | `15721` | `66172` |
| **45%** | [Node (Default)](#node-default) | `45409` | `14371` | `90132` |
| **41%** | [Hono](#hono) | `40844` | `15186` | `59633` |
| **40%** | [Koa](#koa) | `39955` | `17036` | `114822` |
| **12%** | [Carbon](#carbon) | `11698` | `2439` | `16999` |
| **9%** | [Express](#express) | `9229` | `1952` | `13595` |


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
    Reqs/sec     12841.03    9323.44   97394.90
    Latency        3.88ms     3.38ms   289.29ms
    HTTP codes:
      1xx - 0, 2xx - 89865, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10135
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10135
    Throughput:     2.62MB/s
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
    Reqs/sec     10702.43    9126.48   94762.24
    Latency        4.66ms     3.04ms   277.34ms
    HTTP codes:
      1xx - 0, 2xx - 89277, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10723
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10703
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 20
    Throughput:     2.74MB/s
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
    Reqs/sec     48186.67   15983.96   66976.21
    Latency        1.04ms     1.38ms   122.85ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    10.94MB/s
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
    Reqs/sec     39201.46   13044.79   57922.22
    Latency        1.27ms     1.65ms   143.49ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.87MB/s
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
    Reqs/sec     89875.59    3968.58   93878.63
    Latency      554.64us    44.67us     2.58ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    12.77MB/s
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
    Reqs/sec     37972.11   15268.56   92859.90
    Latency        1.31ms     1.79ms   149.02ms
    HTTP codes:
      1xx - 0, 2xx - 91500, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8500
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8500
    Throughput:     7.87MB/s
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
    Reqs/sec     46749.35   14637.82   76044.66
    Latency        1.07ms     1.34ms   113.54ms
    HTTP codes:
      1xx - 0, 2xx - 97225, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2775
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2775
    Throughput:    10.38MB/s
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
    Reqs/sec    101030.11    3658.22  108937.91
    Latency      493.08us   115.28us     4.60ms
    HTTP codes:
      1xx - 0, 2xx - 97145, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2855
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2855
    Throughput:    15.53MB/s
  ```


