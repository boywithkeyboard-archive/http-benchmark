## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75671` | `6536` | `106469` |
| **83%** | [Hyper Express](#hyper-express) | `62508` | `3863` | `66019` |
| **36%** | [Node (Default)](#node-default) | `26883` | `7605` | `51499` |
| **33%** | [Fastify](#fastify) | `24768` | `7648` | `38079` |
| **29%** | [Hono](#hono) | `21598` | `6297` | `30369` |
| **25%** | [Koa](#koa) | `18823` | `7040` | `60095` |
| **11%** | [Carbon](#carbon) | `8492` | `1449` | `10512` |
| **9%** | [Express](#express) | `6481` | `997` | `8441` |


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
    Reqs/sec      8853.39    3598.29   50493.43
    Latency        5.64ms     4.23ms   367.29ms
    HTTP codes:
      1xx - 0, 2xx - 95632, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4368
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4368
    Throughput:     1.92MB/s
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
    Reqs/sec      6800.44    4060.14   53452.11
    Latency        7.34ms     3.88ms   356.00ms
    HTTP codes:
      1xx - 0, 2xx - 92939, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7061
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7057
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 4
    Throughput:     1.81MB/s
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
    Reqs/sec     25407.72    8067.58   38376.74
    Latency        1.96ms     1.97ms   181.19ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.77MB/s
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
    Reqs/sec     22238.94    5787.85   30199.14
    Latency        2.25ms     1.96ms   178.49ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.03MB/s
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
    Reqs/sec     63106.95    3492.72   68135.02
    Latency      789.29us    65.64us     2.75ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.97MB/s
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
    Reqs/sec     19083.44    6439.87   48415.77
    Latency        2.61ms     2.19ms   191.41ms
    HTTP codes:
      1xx - 0, 2xx - 94487, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5513
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5513
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
    Reqs/sec     28151.00    8326.38   56249.85
    Latency        1.77ms     1.82ms   159.37ms
    HTTP codes:
      1xx - 0, 2xx - 97597, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2403
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2403
    Throughput:     6.29MB/s
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
    Reqs/sec     73983.01    5559.15   86806.17
    Latency      673.37us   176.45us     8.99ms
    HTTP codes:
      1xx - 0, 2xx - 96705, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3295
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3295
    Throughput:    11.32MB/s
  ```


