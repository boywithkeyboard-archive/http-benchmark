## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73743` | `5088` | `81470` |
| **86%** | [Hyper Express](#hyper-express) | `63284` | `3751` | `71775` |
| **35%** | [Node (Default)](#node-default) | `26057` | `7669` | `55533` |
| **33%** | [Fastify](#fastify) | `24536` | `7555` | `37461` |
| **28%** | [Hono](#hono) | `20848` | `5860` | `30726` |
| **25%** | [Koa](#koa) | `18145` | `6878` | `57397` |
| **11%** | [Carbon](#carbon) | `8211` | `1405` | `10297` |
| **9%** | [Express](#express) | `6557` | `1067` | `8393` |


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
    Reqs/sec      8953.07    5046.00   58180.38
    Latency        5.58ms     4.25ms   369.35ms
    HTTP codes:
      1xx - 0, 2xx - 92773, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7227
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7227
    Throughput:     1.89MB/s
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
    Reqs/sec      6983.20    4720.82   57886.28
    Latency        7.15ms     3.91ms   355.24ms
    HTTP codes:
      1xx - 0, 2xx - 91114, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8886
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8886
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
    Reqs/sec     24816.45    7479.18   36649.60
    Latency        2.01ms     1.95ms   180.43ms
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
    Reqs/sec     21747.63    6178.58   30446.80
    Latency        2.30ms     2.01ms   179.09ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.91MB/s
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
    Reqs/sec     62784.01    4246.09   75601.17
    Latency      794.34us    75.83us     3.30ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.92MB/s
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
    Reqs/sec     18205.94    6728.04   56276.01
    Latency        2.74ms     2.44ms   211.39ms
    HTTP codes:
      1xx - 0, 2xx - 94064, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5936
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5936
    Throughput:     3.87MB/s
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
    Reqs/sec     26507.41    7384.93   51028.74
    Latency        1.88ms     1.81ms   155.32ms
    HTTP codes:
      1xx - 0, 2xx - 97777, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2223
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2223
    Throughput:     5.93MB/s
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
    Reqs/sec     73864.24    6570.44  104955.82
    Latency      678.70us   156.30us     6.36ms
    HTTP codes:
      1xx - 0, 2xx - 96911, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3089
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3089
    Throughput:    11.25MB/s
  ```


