## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74056` | `5430` | `81302` |
| **86%** | [Hyper Express](#hyper-express) | `63474` | `3702` | `66935` |
| **34%** | [Node (Default)](#node-default) | `25240` | `6970` | `53709` |
| **32%** | [Fastify](#fastify) | `23338` | `6315` | `36487` |
| **28%** | [Hono](#hono) | `20368` | `5485` | `30020` |
| **25%** | [Koa](#koa) | `18209` | `6050` | `48455` |
| **11%** | [Carbon](#carbon) | `8126` | `1334` | `10471` |
| **9%** | [Express](#express) | `6505` | `1017` | `8577` |


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
    Reqs/sec      8482.99    3697.61   51687.31
    Latency        5.88ms     4.14ms   360.52ms
    HTTP codes:
      1xx - 0, 2xx - 95127, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4873
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4873
    Throughput:     1.83MB/s
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
    Reqs/sec      6568.83    1042.74    8466.55
    Latency        7.61ms     3.51ms   337.56ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.88MB/s
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
    Reqs/sec     24670.12    7878.22   36335.07
    Latency        2.02ms     2.00ms   179.33ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.60MB/s
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
    Reqs/sec     21032.25    5413.64   30316.46
    Latency        2.38ms     2.02ms   182.38ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.75MB/s
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
    Reqs/sec     63192.63    3594.92   70518.49
    Latency      789.19us    67.34us     3.41ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.97MB/s
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
    Reqs/sec     17893.12    6686.17   56031.18
    Latency        2.79ms     2.30ms   201.60ms
    HTTP codes:
      1xx - 0, 2xx - 94131, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5869
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5869
    Throughput:     3.81MB/s
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
    Reqs/sec     25474.48    7385.77   42686.17
    Latency        1.96ms     1.82ms   154.76ms
    HTTP codes:
      1xx - 0, 2xx - 98160, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1840
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1840
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
    Reqs/sec     75618.78    6447.25   81175.66
    Latency      659.65us   223.30us    11.45ms
    HTTP codes:
      1xx - 0, 2xx - 97393, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2607
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2607
    Throughput:    11.64MB/s
  ```


