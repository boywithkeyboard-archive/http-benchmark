## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74401` | `5121` | `83680` |
| **84%** | [Hyper Express](#hyper-express) | `62798` | `3828` | `66390` |
| **35%** | [Node (Default)](#node-default) | `26219` | `7511` | `52184` |
| **32%** | [Fastify](#fastify) | `23648` | `6339` | `36251` |
| **28%** | [Hono](#hono) | `20771` | `5849` | `30904` |
| **25%** | [Koa](#koa) | `18781` | `6726` | `65385` |
| **11%** | [Carbon](#carbon) | `8479` | `1444` | `10541` |
| **9%** | [Express](#express) | `6597` | `1061` | `8609` |


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
    Reqs/sec      8710.14    3748.59   47474.99
    Latency        5.74ms     4.13ms   359.07ms
    HTTP codes:
      1xx - 0, 2xx - 94930, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5070
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5070
    Throughput:     1.88MB/s
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
    Reqs/sec      6693.57    1067.05    8602.70
    Latency        7.47ms     3.46ms   331.51ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.91MB/s
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
    Reqs/sec     24418.89    7208.46   36194.28
    Latency        2.05ms     2.02ms   180.57ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.53MB/s
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
    Reqs/sec     21374.82    6149.39   30170.07
    Latency        2.34ms     1.92ms   172.47ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.83MB/s
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
    Reqs/sec     63742.24    3590.62   69630.13
    Latency      781.92us    73.20us     5.61ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.05MB/s
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
    Reqs/sec     17779.01    6532.78   57112.02
    Latency        2.81ms     2.33ms   201.70ms
    HTTP codes:
      1xx - 0, 2xx - 93914, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6086
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6086
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
    Reqs/sec     25662.94    7684.12   52630.09
    Latency        1.94ms     1.88ms   160.24ms
    HTTP codes:
      1xx - 0, 2xx - 97000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3000
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3000
    Throughput:     5.70MB/s
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
    Reqs/sec     74072.28    5600.89   84568.99
    Latency      672.29us   178.98us    17.35ms
    HTTP codes:
      1xx - 0, 2xx - 98152, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1848
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1848
    Throughput:    11.50MB/s
  ```


