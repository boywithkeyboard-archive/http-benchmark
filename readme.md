## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `71076` | `4429` | `85609` |
| **82%** | [Hyper Express](#hyper-express) | `58510` | `3494` | `65075` |
| **30%** | [Hono](#hono) | `21483` | `6457` | `30777` |
| **29%** | [Node (Default)](#node-default) | `20870` | `5123` | `61386` |
| **29%** | [Fastify](#fastify) | `20665` | `5263` | `36548` |
| **26%** | [Koa](#koa) | `18746` | `9164` | `78103` |
| **10%** | [Carbon](#carbon) | `7406` | `1257` | `10564` |
| **9%** | [Express](#express) | `6113` | `1010` | `8168` |


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
    Reqs/sec      8459.07    6831.82   78274.61
    Latency        5.90ms     4.66ms   392.26ms
    HTTP codes:
      1xx - 0, 2xx - 88911, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11089
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11089
    Throughput:     1.71MB/s
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
    Reqs/sec      6242.06    1099.26    8384.14
    Latency        8.01ms     3.85ms   368.11ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     21186.06    5473.12   36109.72
    Latency        2.36ms     2.03ms   182.27ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.80MB/s
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
    Reqs/sec     21380.16    6739.99   30939.21
    Latency        2.34ms     2.38ms   210.95ms
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
    Reqs/sec     58271.05    3450.03   63777.18
    Latency        0.86ms    98.54us     4.87ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.28MB/s
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
    Reqs/sec     18173.56    8177.94   64846.37
    Latency        2.74ms     2.42ms   214.29ms
    HTTP codes:
      1xx - 0, 2xx - 92809, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7191
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7191
    Throughput:     3.82MB/s
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
    Reqs/sec     20653.58    5085.49   60780.30
    Latency        2.42ms     2.02ms   173.42ms
    HTTP codes:
      1xx - 0, 2xx - 97192, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2808
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2808
    Throughput:     4.60MB/s
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
    Reqs/sec     70372.02    9164.00   82308.01
    Latency      698.01us   188.05us     8.85ms
    HTTP codes:
      1xx - 0, 2xx - 96474, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3526
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3526
    Throughput:    10.90MB/s
  ```


