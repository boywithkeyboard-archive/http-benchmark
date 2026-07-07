## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `67780` | `4160` | `86487` |
| **85%** | [Hyper Express](#hyper-express) | `57500` | `4429` | `68255` |
| **30%** | [Hono](#hono) | `20423` | `6460` | `29786` |
| **28%** | [Fastify](#fastify) | `19132` | `4353` | `35127` |
| **28%** | [Node (Default)](#node-default) | `19107` | `4486` | `64140` |
| **27%** | [Koa](#koa) | `18318` | `8076` | `64465` |
| **11%** | [Carbon](#carbon) | `7509` | `1368` | `10438` |
| **9%** | [Express](#express) | `5975` | `1041` | `8308` |


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
    Reqs/sec      7869.99    4650.77   56610.68
    Latency        6.35ms     4.92ms   413.43ms
    HTTP codes:
      1xx - 0, 2xx - 93202, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6798
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6798
    Throughput:     1.66MB/s
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
    Reqs/sec      5916.56    1074.27    8267.29
    Latency        8.45ms     4.08ms   387.17ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.69MB/s
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
    Reqs/sec     20344.77    5365.04   35597.00
    Latency        2.46ms     2.13ms   192.77ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.62MB/s
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
    Reqs/sec     21115.42    6929.95   30443.92
    Latency        2.36ms     2.31ms   205.55ms
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
    Reqs/sec     57173.77    3727.62   63339.97
    Latency        0.87ms   104.45us     4.52ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.12MB/s
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
    Reqs/sec     19266.69    9055.04   62071.53
    Latency        2.59ms     2.71ms   233.94ms
    HTTP codes:
      1xx - 0, 2xx - 90671, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9329
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9329
    Throughput:     3.95MB/s
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
    Reqs/sec     20207.30    5552.11   66657.89
    Latency        2.47ms     2.06ms   176.57ms
    HTTP codes:
      1xx - 0, 2xx - 96632, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3368
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3368
    Throughput:     4.47MB/s
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
    Reqs/sec     70812.62    4952.61   81840.08
    Latency      703.73us   177.73us     7.29ms
    HTTP codes:
      1xx - 0, 2xx - 97689, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2311
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2311
    Throughput:    10.95MB/s
  ```


