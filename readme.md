## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72874` | `7225` | `83865` |
| **85%** | [Hyper Express](#hyper-express) | `61880` | `3521` | `65866` |
| **36%** | [Node (Default)](#node-default) | `25909` | `7888` | `54178` |
| **32%** | [Fastify](#fastify) | `23483` | `6930` | `36704` |
| **27%** | [Hono](#hono) | `19505` | `5436` | `29953` |
| **26%** | [Koa](#koa) | `19028` | `7104` | `54305` |
| **11%** | [Carbon](#carbon) | `8256` | `1483` | `10479` |
| **9%** | [Express](#express) | `6265` | `971` | `8363` |


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
    Reqs/sec      8557.42    4649.08   54621.79
    Latency        5.83ms     4.28ms   369.73ms
    HTTP codes:
      1xx - 0, 2xx - 92671, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7329
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7329
    Throughput:     1.80MB/s
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
    Reqs/sec      6414.60    1063.94    8454.41
    Latency        7.79ms     3.73ms   356.52ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.83MB/s
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
    Reqs/sec     23326.29    6926.04   36363.24
    Latency        2.14ms     2.08ms   183.00ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.30MB/s
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
    Reqs/sec     20800.57    5929.24   30048.79
    Latency        2.40ms     2.05ms   185.14ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.70MB/s
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
    Reqs/sec     61654.36    3468.23   65469.03
    Latency      809.29us    74.70us     3.24ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.75MB/s
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
    Reqs/sec     18294.60    6717.10   59585.78
    Latency        2.73ms     2.43ms   214.30ms
    HTTP codes:
      1xx - 0, 2xx - 93920, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6080
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6080
    Throughput:     3.88MB/s
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
    Reqs/sec     25597.02    7486.13   55621.16
    Latency        1.95ms     1.94ms   167.43ms
    HTTP codes:
      1xx - 0, 2xx - 97709, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2291
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2291
    Throughput:     5.73MB/s
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
    Reqs/sec     72873.75    4264.33   76643.55
    Latency      684.06us   138.27us     4.71ms
    HTTP codes:
      1xx - 0, 2xx - 97348, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2652
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2652
    Throughput:    11.22MB/s
  ```


