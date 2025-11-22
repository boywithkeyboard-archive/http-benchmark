## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74789` | `4195` | `85327` |
| **82%** | [Hyper Express](#hyper-express) | `61627` | `3613` | `64042` |
| **33%** | [Node (Default)](#node-default) | `24927` | `8071` | `73456` |
| **31%** | [Fastify](#fastify) | `23389` | `6864` | `35104` |
| **27%** | [Hono](#hono) | `20144` | `5587` | `29682` |
| **25%** | [Koa](#koa) | `18702` | `8330` | `72708` |
| **11%** | [Carbon](#carbon) | `8144` | `1451` | `10293` |
| **8%** | [Express](#express) | `6325` | `1071` | `8171` |


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
    Reqs/sec      8561.82    5662.54   75063.18
    Latency        5.83ms     4.61ms   393.56ms
    HTTP codes:
      1xx - 0, 2xx - 91881, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8119
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8119
    Throughput:     1.79MB/s
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
    Reqs/sec      6046.93     985.57    8074.72
    Latency        8.27ms     3.75ms   363.50ms
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
    Reqs/sec     23454.61    6964.28   34661.62
    Latency        2.13ms     2.12ms   188.24ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.32MB/s
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
    Reqs/sec     20492.48    6240.12   30026.64
    Latency        2.44ms     2.08ms   186.21ms
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
    Reqs/sec     62834.44    3683.39   69039.55
    Latency      793.10us    68.45us     3.09ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.93MB/s
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
    Reqs/sec     18700.04    8032.24   66135.86
    Latency        2.67ms     2.42ms   212.73ms
    HTTP codes:
      1xx - 0, 2xx - 92844, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7156
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7156
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
    Reqs/sec     25782.09    8118.79   67330.46
    Latency        1.93ms     1.89ms   163.59ms
    HTTP codes:
      1xx - 0, 2xx - 96753, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3247
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3247
    Throughput:     5.71MB/s
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
    Reqs/sec     74300.93    3099.16   81057.25
    Latency      670.51us   153.04us     7.63ms
    HTTP codes:
      1xx - 0, 2xx - 97072, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2928
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2928
    Throughput:    11.42MB/s
  ```


