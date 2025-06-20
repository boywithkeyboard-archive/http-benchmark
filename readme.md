## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73461` | `4325` | `79480` |
| **86%** | [Hyper Express](#hyper-express) | `63441` | `4308` | `78894` |
| **36%** | [Node (Default)](#node-default) | `26782` | `7689` | `58745` |
| **32%** | [Fastify](#fastify) | `23820` | `7271` | `36880` |
| **29%** | [Hono](#hono) | `21275` | `6236` | `29750` |
| **26%** | [Koa](#koa) | `19323` | `7822` | `59341` |
| **11%** | [Carbon](#carbon) | `8237` | `1394` | `10488` |
| **9%** | [Express](#express) | `6358` | `1031` | `8457` |


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
    Reqs/sec      8629.04    3618.45   52666.00
    Latency        5.78ms     4.27ms   370.65ms
    HTTP codes:
      1xx - 0, 2xx - 95442, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4558
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4558
    Throughput:     1.87MB/s
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
    Reqs/sec      7052.55    4749.67   57178.30
    Latency        7.08ms     3.91ms   357.56ms
    HTTP codes:
      1xx - 0, 2xx - 91369, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8631
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8631
    Throughput:     1.85MB/s
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
    Reqs/sec     23750.08    7004.64   36532.18
    Latency        2.10ms     2.14ms   190.39ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.39MB/s
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
    Reqs/sec     20509.91    5410.73   30389.17
    Latency        2.44ms     2.07ms   188.12ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.63MB/s
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
    Reqs/sec     62497.31    2914.87   65351.60
    Latency      797.73us    68.06us     4.35ms
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
    Reqs/sec     18692.62    6848.51   55768.93
    Latency        2.67ms     2.49ms   214.79ms
    HTTP codes:
      1xx - 0, 2xx - 93872, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6128
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6128
    Throughput:     3.97MB/s
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
    Reqs/sec     25598.67    8043.07   65327.00
    Latency        1.95ms     2.00ms   171.09ms
    HTTP codes:
      1xx - 0, 2xx - 96317, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3683
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3683
    Throughput:     5.64MB/s
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
    Reqs/sec     74010.98    5371.36   89221.88
    Latency      673.82us   177.29us     7.58ms
    HTTP codes:
      1xx - 0, 2xx - 97601, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2399
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2399
    Throughput:    11.42MB/s
  ```


