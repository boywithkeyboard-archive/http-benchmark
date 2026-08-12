## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `69457` | `3830` | `82474` |
| **86%** | [Hyper Express](#hyper-express) | `59785` | `4760` | `83764` |
| **29%** | [Node (Default)](#node-default) | `19983` | `4673` | `55038` |
| **29%** | [Hono](#hono) | `19866` | `6305` | `31487` |
| **28%** | [Fastify](#fastify) | `19408` | `3889` | `33598` |
| **26%** | [Koa](#koa) | `18406` | `7836` | `66476` |
| **11%** | [Carbon](#carbon) | `7473` | `1334` | `10491` |
| **9%** | [Express](#express) | `6182` | `1092` | `8279` |


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
    Reqs/sec      7695.00    4930.01   61059.79
    Latency        6.49ms     4.53ms   387.65ms
    HTTP codes:
      1xx - 0, 2xx - 92425, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7575
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7575
    Throughput:     1.62MB/s
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
    Reqs/sec      6251.84    1106.29    8352.81
    Latency        7.99ms     3.93ms   374.15ms
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
    Reqs/sec     19504.58    3903.00   33393.27
    Latency        2.56ms     2.06ms   184.18ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.43MB/s
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
    Reqs/sec     21153.22    6651.47   30894.00
    Latency        2.36ms     2.11ms   189.32ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.77MB/s
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
    Reqs/sec     59254.89    3417.57   65966.06
    Latency      841.72us    99.11us     4.42ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.42MB/s
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
    Reqs/sec     18597.01    8536.23   69154.58
    Latency        2.68ms     2.41ms   207.01ms
    HTTP codes:
      1xx - 0, 2xx - 92649, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7351
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7351
    Throughput:     3.89MB/s
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
    Reqs/sec     20615.45    5479.63   66169.10
    Latency        2.42ms     2.03ms   179.99ms
    HTTP codes:
      1xx - 0, 2xx - 96681, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3319
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3319
    Throughput:     4.57MB/s
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
    Reqs/sec     70492.95    4368.42   85887.28
    Latency      706.58us   174.90us     9.90ms
    HTTP codes:
      1xx - 0, 2xx - 95449, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4551
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4551
    Throughput:    10.65MB/s
  ```


