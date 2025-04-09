## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74055` | `5161` | `85255` |
| **85%** | [Hyper Express](#hyper-express) | `63261` | `3613` | `68023` |
| **38%** | [Node (Default)](#node-default) | `28246` | `8516` | `51532` |
| **33%** | [Fastify](#fastify) | `24775` | `7361` | `36569` |
| **28%** | [Hono](#hono) | `20983` | `6203` | `30259` |
| **26%** | [Koa](#koa) | `19007` | `7542` | `58123` |
| **11%** | [Carbon](#carbon) | `7999` | `1330` | `10138` |
| **9%** | [Express](#express) | `6505` | `1044` | `8353` |


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
    Reqs/sec      8894.79    4949.84   54456.50
    Latency        5.60ms     4.27ms   368.84ms
    HTTP codes:
      1xx - 0, 2xx - 91737, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8263
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8263
    Throughput:     1.86MB/s
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
    Reqs/sec      7054.25    4554.31   58187.63
    Latency        7.08ms     3.88ms   356.91ms
    HTTP codes:
      1xx - 0, 2xx - 92047, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7953
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7953
    Throughput:     1.86MB/s
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
    Reqs/sec     24835.07    7690.32   36306.40
    Latency        2.01ms     2.01ms   180.28ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.64MB/s
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
    Reqs/sec     21561.93    5926.41   30512.56
    Latency        2.32ms     2.02ms   183.32ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.87MB/s
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
    Reqs/sec     63196.21    3798.74   70493.01
    Latency      789.09us    66.10us     2.82ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.98MB/s
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
    Reqs/sec     19390.16    6812.88   57223.18
    Latency        2.57ms     2.38ms   208.27ms
    HTTP codes:
      1xx - 0, 2xx - 93560, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6440
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6440
    Throughput:     4.10MB/s
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
    Reqs/sec     25657.96    7389.04   53684.63
    Latency        1.94ms     1.87ms   162.95ms
    HTTP codes:
      1xx - 0, 2xx - 97561, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2439
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2439
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
    Reqs/sec     73285.76    5183.62   88131.29
    Latency      680.58us   179.58us     9.32ms
    HTTP codes:
      1xx - 0, 2xx - 97001, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2999
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2999
    Throughput:    11.24MB/s
  ```


