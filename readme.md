## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74024` | `2487` | `78229` |
| **84%** | [Hyper Express](#hyper-express) | `62309` | `3270` | `65590` |
| **35%** | [Node (Default)](#node-default) | `25890` | `9720` | `78049` |
| **33%** | [Fastify](#fastify) | `24211` | `7532` | `34985` |
| **27%** | [Hono](#hono) | `19673` | `5475` | `29343` |
| **26%** | [Koa](#koa) | `19463` | `8357` | `69986` |
| **11%** | [Carbon](#carbon) | `7993` | `1395` | `10201` |
| **9%** | [Express](#express) | `6501` | `1081` | `8386` |


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
    Reqs/sec      8909.62    6018.51   68508.46
    Latency        5.60ms     4.56ms   390.98ms
    HTTP codes:
      1xx - 0, 2xx - 91370, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8630
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8630
    Throughput:     1.85MB/s
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
    Reqs/sec      6968.90    6105.71   73702.54
    Latency        7.17ms     3.96ms   365.35ms
    HTTP codes:
      1xx - 0, 2xx - 89519, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10481
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10481
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
    Reqs/sec     23775.48    7113.07   34394.37
    Latency        2.10ms     2.05ms   184.41ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.39MB/s
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
    Reqs/sec     21380.47    6180.82   30309.52
    Latency        2.34ms     2.02ms   183.19ms
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
    Reqs/sec     62265.09    3552.63   65433.55
    Latency      799.29us    66.34us     3.42ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.86MB/s
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
    Reqs/sec     18515.54    8295.31   77465.66
    Latency        2.70ms     2.39ms   215.07ms
    HTTP codes:
      1xx - 0, 2xx - 92808, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7192
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7192
    Throughput:     3.88MB/s
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
    Reqs/sec     25230.90    7786.29   69178.07
    Latency        1.97ms     1.85ms   162.98ms
    HTTP codes:
      1xx - 0, 2xx - 97055, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2945
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2945
    Throughput:     5.62MB/s
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
    Reqs/sec     74612.30    2938.31   85041.25
    Latency      669.05us   147.29us     6.44ms
    HTTP codes:
      1xx - 0, 2xx - 97503, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2497
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2497
    Throughput:    11.49MB/s
  ```


