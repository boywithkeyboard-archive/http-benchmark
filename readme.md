## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73833` | `3140` | `82570` |
| **82%** | [Hyper Express](#hyper-express) | `60518` | `3250` | `64191` |
| **33%** | [Node (Default)](#node-default) | `24525` | `7938` | `69344` |
| **31%** | [Fastify](#fastify) | `23026` | `7242` | `34056` |
| **29%** | [Hono](#hono) | `21436` | `6526` | `29320` |
| **25%** | [Koa](#koa) | `18810` | `9381` | `82221` |
| **11%** | [Carbon](#carbon) | `7850` | `1333` | `10154` |
| **8%** | [Express](#express) | `6105` | `1012` | `8184` |


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
    Reqs/sec      8326.45    5154.86   77824.60
    Latency        6.00ms     4.50ms   387.14ms
    HTTP codes:
      1xx - 0, 2xx - 93177, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6823
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6823
    Throughput:     1.76MB/s
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
    Reqs/sec      6111.79    1022.89    8051.60
    Latency        8.18ms     3.79ms   364.78ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.75MB/s
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
    Reqs/sec     23195.68    6800.94   34300.69
    Latency        2.15ms     1.95ms   177.07ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.26MB/s
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
    Reqs/sec     21491.61    6516.68   29061.25
    Latency        2.33ms     2.15ms   189.22ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.85MB/s
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
    Reqs/sec     61111.89    3229.51   64337.19
    Latency      815.90us    73.94us     2.77ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.68MB/s
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
    Reqs/sec     18330.02    9110.00   81648.03
    Latency        2.72ms     2.57ms   226.79ms
    HTTP codes:
      1xx - 0, 2xx - 91003, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8997
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8997
    Throughput:     3.77MB/s
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
    Reqs/sec     24412.69    8056.62   76442.01
    Latency        2.04ms     2.15ms   186.20ms
    HTTP codes:
      1xx - 0, 2xx - 96423, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3577
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3577
    Throughput:     5.40MB/s
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
    Reqs/sec     73541.99    2763.10   80416.93
    Latency      676.23us   205.78us    10.81ms
    HTTP codes:
      1xx - 0, 2xx - 95998, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4002
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4002
    Throughput:    11.17MB/s
  ```


