## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `70836` | `4198` | `82579` |
| **82%** | [Hyper Express](#hyper-express) | `58369` | `3521` | `66793` |
| **34%** | [Node (Default)](#node-default) | `23940` | `7635` | `72908` |
| **32%** | [Fastify](#fastify) | `22585` | `6415` | `36266` |
| **29%** | [Hono](#hono) | `20466` | `6461` | `30796` |
| **27%** | [Koa](#koa) | `19060` | `8218` | `61187` |
| **11%** | [Carbon](#carbon) | `8065` | `1439` | `10170` |
| **9%** | [Express](#express) | `6147` | `1018` | `8238` |


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
    Reqs/sec      8762.17    5465.37   64335.76
    Latency        5.70ms     4.52ms   391.73ms
    HTTP codes:
      1xx - 0, 2xx - 92225, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7775
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7775
    Throughput:     1.84MB/s
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
    Reqs/sec      6055.98     959.03    8083.84
    Latency        8.25ms     3.72ms   359.24ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.73MB/s
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
    Reqs/sec     21991.84    6549.29   35199.67
    Latency        2.27ms     2.13ms   190.39ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.99MB/s
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
    Reqs/sec     21666.77    6726.19   30780.17
    Latency        2.31ms     2.12ms   189.78ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.89MB/s
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
    Reqs/sec     57803.88    3410.49   66474.67
    Latency        0.86ms    81.98us     3.13ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.21MB/s
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
    Reqs/sec     18637.91    7950.53   61759.45
    Latency        2.67ms     2.47ms   216.98ms
    HTTP codes:
      1xx - 0, 2xx - 93245, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6755
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6755
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
    Reqs/sec     23624.07    6843.65   65317.71
    Latency        2.11ms     2.04ms   170.50ms
    HTTP codes:
      1xx - 0, 2xx - 96590, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3410
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3410
    Throughput:     5.22MB/s
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
    Reqs/sec     70064.63    3495.67   81763.04
    Latency      710.06us   191.21us    10.89ms
    HTTP codes:
      1xx - 0, 2xx - 96621, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3379
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3379
    Throughput:    10.71MB/s
  ```


