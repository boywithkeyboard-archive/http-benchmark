## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `70581` | `3976` | `79587` |
| **85%** | [Hyper Express](#hyper-express) | `59870` | `3951` | `65367` |
| **30%** | [Hono](#hono) | `20849` | `6430` | `31048` |
| **29%** | [Fastify](#fastify) | `20773` | `5198` | `36909` |
| **29%** | [Node (Default)](#node-default) | `20260` | `5842` | `82525` |
| **28%** | [Koa](#koa) | `19910` | `9103` | `78034` |
| **10%** | [Carbon](#carbon) | `7240` | `1213` | `10386` |
| **9%** | [Express](#express) | `6173` | `1110` | `8274` |


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
    Reqs/sec      8197.23    7784.78   79878.07
    Latency        6.09ms     4.75ms   402.10ms
    HTTP codes:
      1xx - 0, 2xx - 86502, 3xx - 0, 4xx - 0, 5xx - 0
      others - 13498
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 13498
    Throughput:     1.61MB/s
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
    Reqs/sec      6245.55    1163.74    8442.73
    Latency        8.00ms     3.82ms   362.68ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.79MB/s
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
    Reqs/sec     19881.13    5146.32   35988.32
    Latency        2.51ms     1.95ms   177.10ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.51MB/s
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
    Reqs/sec     21253.85    6433.13   31571.14
    Latency        2.35ms     2.17ms   191.20ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.80MB/s
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
    Reqs/sec     58809.42    3164.61   65144.31
    Latency      847.71us    93.31us     3.14ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.35MB/s
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
    Reqs/sec     18335.74    8465.69   70837.05
    Latency        2.72ms     2.44ms   214.16ms
    HTTP codes:
      1xx - 0, 2xx - 92372, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7628
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7628
    Throughput:     3.83MB/s
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
    Reqs/sec     20322.33    5695.86   76891.55
    Latency        2.45ms     1.91ms   171.58ms
    HTTP codes:
      1xx - 0, 2xx - 96372, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3628
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3628
    Throughput:     4.49MB/s
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
    Reqs/sec     69590.68    3038.90   80003.19
    Latency      714.07us   205.23us    12.37ms
    HTTP codes:
      1xx - 0, 2xx - 96425, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3575
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3575
    Throughput:    10.62MB/s
  ```


