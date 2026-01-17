## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73598` | `2374` | `77234` |
| **84%** | [Hyper Express](#hyper-express) | `61711` | `3362` | `65123` |
| **34%** | [Node (Default)](#node-default) | `25038` | `8481` | `59793` |
| **31%** | [Fastify](#fastify) | `23168` | `6867` | `35086` |
| **27%** | [Hono](#hono) | `19704` | `5397` | `28479` |
| **25%** | [Koa](#koa) | `18758` | `9001` | `76471` |
| **11%** | [Carbon](#carbon) | `7915` | `1370` | `10215` |
| **8%** | [Express](#express) | `6110` | `1005` | `8137` |


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
    Reqs/sec      8698.66    6116.18   79100.36
    Latency        5.74ms     4.37ms   376.73ms
    HTTP codes:
      1xx - 0, 2xx - 91521, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8479
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8479
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
    Reqs/sec      6197.49     983.20    8167.73
    Latency        8.06ms     3.80ms   365.18ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.77MB/s
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
    Reqs/sec     22891.82    7151.58   35684.17
    Latency        2.18ms     2.11ms   188.69ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.19MB/s
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
    Reqs/sec     19758.97    5528.79   29020.07
    Latency        2.53ms     2.06ms   184.24ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.47MB/s
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
    Reqs/sec     60219.42    5486.72   70857.33
    Latency      829.58us   115.31us     5.38ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.54MB/s
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
    Reqs/sec     18179.92    8488.45   78421.80
    Latency        2.74ms     2.50ms   219.56ms
    HTTP codes:
      1xx - 0, 2xx - 92114, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7886
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7886
    Throughput:     3.79MB/s
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
    Reqs/sec     25548.73    8357.28   70948.89
    Latency        1.95ms     1.99ms   167.84ms
    HTTP codes:
      1xx - 0, 2xx - 96383, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3617
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3617
    Throughput:     5.63MB/s
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
    Reqs/sec     73963.02    2799.93   80377.21
    Latency      673.22us   153.71us     8.86ms
    HTTP codes:
      1xx - 0, 2xx - 97207, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2793
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2793
    Throughput:    11.38MB/s
  ```


