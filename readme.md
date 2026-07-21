## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `70858` | `4281` | `82375` |
| **83%** | [Hyper Express](#hyper-express) | `58492` | `3437` | `63534` |
| **29%** | [Hono](#hono) | `20328` | `6221` | `30641` |
| **29%** | [Node (Default)](#node-default) | `20247` | `5230` | `70411` |
| **28%** | [Fastify](#fastify) | `20116` | `5149` | `35338` |
| **26%** | [Koa](#koa) | `18372` | `8100` | `63771` |
| **10%** | [Carbon](#carbon) | `7323` | `1250` | `10349` |
| **9%** | [Express](#express) | `6136` | `1087` | `8182` |


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
    Reqs/sec      7922.97    6779.19   74539.52
    Latency        6.30ms     4.60ms   386.86ms
    HTTP codes:
      1xx - 0, 2xx - 88709, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11291
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11291
    Throughput:     1.60MB/s
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
    Reqs/sec      6193.07    1106.09    8257.98
    Latency        8.07ms     4.02ms   382.72ms
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
    Reqs/sec     18882.07    3991.03   35075.96
    Latency        2.65ms     2.06ms   188.38ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.28MB/s
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
    Reqs/sec     21152.93    6604.71   30708.42
    Latency        2.36ms     2.37ms   202.77ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.78MB/s
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
    Reqs/sec     58567.95    3848.94   65756.19
    Latency        0.85ms   104.99us     3.35ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.32MB/s
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
    Reqs/sec     17030.20    7960.80   66409.90
    Latency        2.93ms     2.68ms   234.12ms
    HTTP codes:
      1xx - 0, 2xx - 92749, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7251
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7251
    Throughput:     3.57MB/s
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
    Reqs/sec     19516.45    4967.01   63682.28
    Latency        2.56ms     2.02ms   173.44ms
    HTTP codes:
      1xx - 0, 2xx - 96898, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3102
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3102
    Throughput:     4.33MB/s
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
    Reqs/sec     69845.31    4700.65   79334.20
    Latency      713.00us   220.89us    12.93ms
    HTTP codes:
      1xx - 0, 2xx - 96333, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3667
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3667
    Throughput:    10.64MB/s
  ```


