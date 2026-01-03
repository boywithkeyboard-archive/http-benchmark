## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74943` | `2749` | `80057` |
| **84%** | [Hyper Express](#hyper-express) | `62727` | `4572` | `86314` |
| **36%** | [Node (Default)](#node-default) | `27088` | `9639` | `79423` |
| **33%** | [Fastify](#fastify) | `24521` | `7949` | `36864` |
| **28%** | [Hono](#hono) | `21213` | `6062` | `29880` |
| **26%** | [Koa](#koa) | `19777` | `9366` | `81517` |
| **11%** | [Carbon](#carbon) | `8244` | `1377` | `10282` |
| **8%** | [Express](#express) | `6359` | `1067` | `8284` |


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
    Reqs/sec      9042.93    6738.17   77955.39
    Latency        5.52ms     4.28ms   368.12ms
    HTTP codes:
      1xx - 0, 2xx - 90460, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9540
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9540
    Throughput:     1.86MB/s
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
    Reqs/sec      6964.60    5478.14   69370.52
    Latency        7.17ms     3.92ms   362.75ms
    HTTP codes:
      1xx - 0, 2xx - 90835, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9165
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9165
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
    Reqs/sec     24537.00    7854.99   36131.21
    Latency        2.04ms     2.02ms   182.54ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.57MB/s
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
    Reqs/sec     20752.08    5915.23   29893.33
    Latency        2.41ms     2.04ms   181.27ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.69MB/s
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
    Reqs/sec     62582.32    4106.22   77503.16
    Latency      799.11us    68.77us     3.28ms
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
    Reqs/sec     19589.10   10457.09   80308.12
    Latency        2.55ms     2.39ms   209.65ms
    HTTP codes:
      1xx - 0, 2xx - 88736, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11264
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11264
    Throughput:     3.93MB/s
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
    Reqs/sec     25134.56    8572.47   80812.79
    Latency        1.99ms     1.95ms   165.40ms
    HTTP codes:
      1xx - 0, 2xx - 96584, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3416
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3416
    Throughput:     5.56MB/s
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
    Reqs/sec     73155.99    2660.51   81514.73
    Latency      681.19us   141.56us     7.31ms
    HTTP codes:
      1xx - 0, 2xx - 97250, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2750
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2750
    Throughput:    11.26MB/s
  ```


