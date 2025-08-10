## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73651` | `2743` | `76980` |
| **84%** | [Hyper Express](#hyper-express) | `61999` | `3346` | `66087` |
| **37%** | [Node (Default)](#node-default) | `26898` | `8937` | `72455` |
| **32%** | [Fastify](#fastify) | `23606` | `7285` | `35969` |
| **28%** | [Hono](#hono) | `20346` | `5467` | `29456` |
| **25%** | [Koa](#koa) | `18516` | `8534` | `74051` |
| **11%** | [Carbon](#carbon) | `8186` | `1459` | `10268` |
| **9%** | [Express](#express) | `6360` | `1039` | `8455` |


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
    Reqs/sec      8803.03    5652.11   70997.37
    Latency        5.67ms     4.37ms   376.03ms
    HTTP codes:
      1xx - 0, 2xx - 91935, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8065
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8065
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
    Reqs/sec      7027.75    6258.59   78421.46
    Latency        7.10ms     3.95ms   358.16ms
    HTTP codes:
      1xx - 0, 2xx - 89287, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10713
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10713
    Throughput:     1.80MB/s
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
    Reqs/sec     23932.93    7591.46   36739.23
    Latency        2.09ms     2.01ms   179.99ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.43MB/s
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
    Reqs/sec     20478.69    5918.32   30622.30
    Latency        2.44ms     2.10ms   188.50ms
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
    Reqs/sec     62504.85    2605.49   65281.13
    Latency      797.63us    66.80us     2.71ms
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
    Reqs/sec     19388.37    7843.31   62059.79
    Latency        2.57ms     2.38ms   209.15ms
    HTTP codes:
      1xx - 0, 2xx - 93495, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6505
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6505
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
    Reqs/sec     26177.44    8027.75   62268.93
    Latency        1.90ms     1.98ms   167.64ms
    HTTP codes:
      1xx - 0, 2xx - 97375, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2625
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2625
    Throughput:     5.85MB/s
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
    Reqs/sec     74661.90    3737.73   89419.90
    Latency      667.25us   138.12us     4.96ms
    HTTP codes:
      1xx - 0, 2xx - 97024, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2976
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2976
    Throughput:    11.47MB/s
  ```


