## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `82975` | `19452` | `110677` |
| **97%** | [Hyper Express](#hyper-express) | `80338` | `15431` | `104497` |
| **59%** | [Node (Default)](#node-default) | `49097` | `15550` | `119130` |
| **57%** | [Fastify](#fastify) | `47312` | `13986` | `64556` |
| **57%** | [Hono](#hono) | `46907` | `12579` | `60084` |
| **49%** | [Koa](#koa) | `40912` | `13074` | `92883` |
| **20%** | [Carbon](#carbon) | `16365` | `5847` | `26715` |
| **16%** | [Express](#express) | `13630` | `3421` | `18991` |


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
    Reqs/sec     17499.85    7705.72   80475.55
    Latency        2.84ms     4.11ms   315.13ms
    HTTP codes:
      1xx - 0, 2xx - 96045, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3955
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3955
    Throughput:     3.82MB/s
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
    Reqs/sec     13599.88    9342.81   91922.33
    Latency        3.67ms     3.58ms   239.25ms
    HTTP codes:
      1xx - 0, 2xx - 91392, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8608
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8608
    Throughput:     3.55MB/s
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
    Reqs/sec     43785.88   16430.32   75886.50
    Latency        1.14ms     1.58ms   121.64ms
    HTTP codes:
      1xx - 0, 2xx - 91560, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8440
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8440
    Throughput:     9.09MB/s
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
    Reqs/sec     38124.20   18337.16   96754.28
    Latency        1.31ms     1.82ms   130.59ms
    HTTP codes:
      1xx - 0, 2xx - 92473, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7527
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7527
    Throughput:     7.94MB/s
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
    Reqs/sec     70070.98   21165.29  126363.66
    Latency      711.36us   542.28us    14.25ms
    HTTP codes:
      1xx - 0, 2xx - 93248, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6752
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6752
    Throughput:     9.26MB/s
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
    Reqs/sec     33631.58   15419.39   89752.42
    Latency        1.48ms     1.16ms    77.38ms
    HTTP codes:
      1xx - 0, 2xx - 89942, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10058
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10058
    Throughput:     6.83MB/s
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
    Reqs/sec     40546.48   14691.20   80571.75
    Latency        1.23ms     1.63ms    99.06ms
    HTTP codes:
      1xx - 0, 2xx - 96601, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3399
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3399
    Throughput:     8.95MB/s
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
    Reqs/sec     79788.37   15113.86  110210.35
    Latency      623.83us   481.44us     8.83ms
    HTTP codes:
      1xx - 0, 2xx - 97002, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2998
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2998
    Throughput:    12.25MB/s
  ```


