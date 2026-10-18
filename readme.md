## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `112263` | `6687` | `131261` |
| **82%** | [Hyper Express](#hyper-express) | `92444` | `6181` | `100711` |
| **32%** | [Node (Default)](#node-default) | `36401` | `9828` | `88056` |
| **30%** | [Fastify](#fastify) | `33739` | `7564` | `52230` |
| **26%** | [Koa](#koa) | `29167` | `13313` | `97168` |
| **26%** | [Hono](#hono) | `28666` | `7338` | `46049` |
| **10%** | [Carbon](#carbon) | `11307` | `2223` | `15959` |
| **7%** | [Express](#express) | `8368` | `1641` | `11496` |


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
    Reqs/sec     12778.81    9012.83  101809.36
    Latency        3.90ms     4.05ms   340.67ms
    HTTP codes:
      1xx - 0, 2xx - 90695, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9305
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9305
    Throughput:     2.64MB/s
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
    Reqs/sec      9543.07   10395.87  106696.14
    Latency        5.22ms     3.40ms   312.83ms
    HTTP codes:
      1xx - 0, 2xx - 85713, 3xx - 0, 4xx - 0, 5xx - 0
      others - 14287
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 14285
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 2
    Throughput:     2.35MB/s
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
    Reqs/sec     40002.78   21202.60  120724.28
    Latency        1.25ms     1.57ms   130.79ms
    HTTP codes:
      1xx - 0, 2xx - 73562, 3xx - 0, 4xx - 0, 5xx - 0
      others - 26438
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 26438
    Throughput:     6.67MB/s
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
    Reqs/sec     31946.03   12616.46  100226.27
    Latency        1.56ms     1.80ms   146.39ms
    HTTP codes:
      1xx - 0, 2xx - 90852, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9148
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9148
    Throughput:     6.56MB/s
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
    Reqs/sec     94038.42    5954.21  100381.73
    Latency      529.73us   203.50us     7.50ms
    HTTP codes:
      1xx - 0, 2xx - 92101, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7899
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7899
    Throughput:    12.29MB/s
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
    Reqs/sec     29992.97   12818.79  100842.49
    Latency        1.67ms     1.97ms   165.13ms
    HTTP codes:
      1xx - 0, 2xx - 90966, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9034
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9034
    Throughput:     6.15MB/s
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
    Reqs/sec     36637.55   10374.38   97895.02
    Latency        1.36ms     1.49ms   126.27ms
    HTTP codes:
      1xx - 0, 2xx - 95805, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4195
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4195
    Throughput:     8.04MB/s
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
    Reqs/sec    112070.73    6622.99  124006.37
    Latency      443.71us   124.51us     5.80ms
    HTTP codes:
      1xx - 0, 2xx - 96707, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3293
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3293
    Throughput:    17.15MB/s
  ```


