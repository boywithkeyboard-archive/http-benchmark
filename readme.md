## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74035` | `5246` | `84091` |
| **85%** | [Hyper Express](#hyper-express) | `63298` | `4149` | `71008` |
| **37%** | [Node (Default)](#node-default) | `27072` | `7774` | `53381` |
| **33%** | [Fastify](#fastify) | `24236` | `7624` | `36697` |
| **28%** | [Hono](#hono) | `20462` | `5459` | `30013` |
| **25%** | [Koa](#koa) | `18812` | `6536` | `52733` |
| **11%** | [Carbon](#carbon) | `8465` | `1474` | `10426` |
| **8%** | [Express](#express) | `6261` | `956` | `8292` |


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
    Reqs/sec      8802.57    4762.85   57660.57
    Latency        5.67ms     4.26ms   370.33ms
    HTTP codes:
      1xx - 0, 2xx - 93028, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6972
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6972
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
    Reqs/sec      6897.76    4452.70   59411.52
    Latency        7.24ms     3.95ms   358.25ms
    HTTP codes:
      1xx - 0, 2xx - 91946, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8054
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8054
    Throughput:     1.82MB/s
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
    Reqs/sec     24320.05    7519.79   37563.12
    Latency        2.05ms     2.31ms   224.56ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.52MB/s
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
    Reqs/sec     20781.87    5409.90   30160.54
    Latency        2.40ms     2.01ms   178.84ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.70MB/s
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
    Reqs/sec     63049.55    4077.56   69778.17
    Latency      790.95us    70.06us     3.13ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.96MB/s
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
    Reqs/sec     19261.31    7184.31   55876.39
    Latency        2.59ms     2.32ms   202.95ms
    HTTP codes:
      1xx - 0, 2xx - 93018, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6982
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6982
    Throughput:     4.05MB/s
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
    Reqs/sec     26104.40    7979.19   54072.02
    Latency        1.91ms     1.92ms   171.05ms
    HTTP codes:
      1xx - 0, 2xx - 96864, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3136
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3136
    Throughput:     5.79MB/s
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
    Reqs/sec     75052.53    6706.39  106756.55
    Latency      667.51us   183.34us     7.56ms
    HTTP codes:
      1xx - 0, 2xx - 96922, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3078
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3078
    Throughput:    11.44MB/s
  ```


