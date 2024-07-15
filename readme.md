## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74367` | `7347` | `91991` |
| **83%** | [Hyper Express](#hyper-express) | `61583` | `2549` | `64505` |
| **36%** | [Node (Default)](#node-default) | `26606` | `8229` | `51027` |
| **32%** | [Fastify](#fastify) | `23458` | `6929` | `36057` |
| **27%** | [Hono](#hono) | `20372` | `5313` | `28928` |
| **24%** | [Koa](#koa) | `17887` | `6445` | `53549` |
| **11%** | [Carbon](#carbon) | `7952` | `1340` | `10240` |
| **9%** | [Express](#express) | `6378` | `971` | `8298` |


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
    Reqs/sec      8385.02    3333.20   52660.99
    Latency        5.95ms     4.30ms   375.15ms
    HTTP codes:
      1xx - 0, 2xx - 95574, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4426
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4426
    Throughput:     1.82MB/s
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
    Reqs/sec      6348.60     995.15    8329.99
    Latency        7.87ms     3.64ms   347.44ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.82MB/s
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
    Reqs/sec     24405.24    7660.66   35658.20
    Latency        2.05ms     2.02ms   179.61ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.54MB/s
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
    Reqs/sec     20023.63    5514.61   29356.80
    Latency        2.50ms     1.99ms   179.55ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.52MB/s
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
    Reqs/sec     62540.21    3286.41   64912.99
    Latency      797.35us    65.99us     2.97ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.88MB/s
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
    Reqs/sec     18069.10    6782.09   56693.84
    Latency        2.76ms     2.45ms   212.80ms
    HTTP codes:
      1xx - 0, 2xx - 93654, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6346
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6346
    Throughput:     3.82MB/s
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
    Reqs/sec     24695.01    7537.49   49737.64
    Latency        2.01ms     1.93ms   165.43ms
    HTTP codes:
      1xx - 0, 2xx - 97821, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2179
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2179
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
    Reqs/sec     74576.35    7081.78   84908.75
    Latency      669.19us   240.02us    10.84ms
    HTTP codes:
      1xx - 0, 2xx - 98373, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1627
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1627
    Throughput:    11.59MB/s
  ```


