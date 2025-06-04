## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74646` | `5036` | `83184` |
| **83%** | [Hyper Express](#hyper-express) | `62233` | `3504` | `65296` |
| **36%** | [Node (Default)](#node-default) | `26729` | `7979` | `50559` |
| **31%** | [Fastify](#fastify) | `23250` | `6657` | `35767` |
| **29%** | [Hono](#hono) | `21632` | `6302` | `30546` |
| **26%** | [Koa](#koa) | `19111` | `6556` | `54273` |
| **11%** | [Carbon](#carbon) | `8365` | `1393` | `10403` |
| **9%** | [Express](#express) | `6549` | `1037` | `8483` |


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
    Reqs/sec      8896.03    4399.84   54765.74
    Latency        5.61ms     4.13ms   354.00ms
    HTTP codes:
      1xx - 0, 2xx - 93673, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6327
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6327
    Throughput:     1.89MB/s
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
    Reqs/sec      7038.70    4499.16   53235.26
    Latency        7.09ms     3.85ms   355.33ms
    HTTP codes:
      1xx - 0, 2xx - 92323, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7677
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7677
    Throughput:     1.86MB/s
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
    Reqs/sec     25261.51    7408.24   36052.38
    Latency        1.98ms     1.91ms   170.56ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.72MB/s
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
    Reqs/sec     21879.26    6249.84   30336.16
    Latency        2.28ms     1.92ms   172.79ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.94MB/s
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
    Reqs/sec     62294.82    3302.26   64552.22
    Latency      799.86us    70.93us     4.01ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.86MB/s
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
    Reqs/sec     19068.93    6451.36   53583.28
    Latency        2.62ms     2.33ms   206.75ms
    HTTP codes:
      1xx - 0, 2xx - 94622, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5378
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5378
    Throughput:     4.08MB/s
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
    Reqs/sec     25757.31    7430.35   56881.99
    Latency        1.93ms     1.81ms   152.43ms
    HTTP codes:
      1xx - 0, 2xx - 96655, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3345
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3345
    Throughput:     5.71MB/s
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
    Reqs/sec     73812.57    5339.51   82247.21
    Latency      674.50us   200.59us     8.38ms
    HTTP codes:
      1xx - 0, 2xx - 96843, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3157
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3157
    Throughput:    11.31MB/s
  ```


