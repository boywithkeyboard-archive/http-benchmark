## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72095` | `5593` | `79700` |
| **86%** | [Hyper Express](#hyper-express) | `62323` | `2366` | `66382` |
| **36%** | [Node (Default)](#node-default) | `25765` | `7366` | `46706` |
| **35%** | [Fastify](#fastify) | `25122` | `7569` | `37084` |
| **30%** | [Hono](#hono) | `21347` | `6089` | `30160` |
| **25%** | [Koa](#koa) | `18153` | `6255` | `53954` |
| **11%** | [Carbon](#carbon) | `8147` | `1374` | `10626` |
| **9%** | [Express](#express) | `6510` | `1032` | `8593` |


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
    Reqs/sec      8657.02    3350.91   46695.92
    Latency        5.77ms     4.11ms   359.61ms
    HTTP codes:
      1xx - 0, 2xx - 95455, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4545
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4545
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
    Reqs/sec      6650.43    1081.57    8539.63
    Latency        7.51ms     3.51ms   336.96ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.90MB/s
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
    Reqs/sec     25390.85    7772.46   37174.55
    Latency        1.97ms     2.02ms   179.09ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.76MB/s
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
    Reqs/sec     21525.26    6140.30   30556.73
    Latency        2.32ms     1.91ms   174.60ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.86MB/s
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
    Reqs/sec     62253.31    4226.89   70793.29
    Latency      798.18us    69.08us     4.08ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.87MB/s
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
    Reqs/sec     18036.19    6144.90   61269.55
    Latency        2.76ms     2.33ms   202.39ms
    HTTP codes:
      1xx - 0, 2xx - 94622, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5378
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5378
    Throughput:     3.86MB/s
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
    Reqs/sec     26185.86    8155.08   57169.00
    Latency        1.90ms     1.97ms   168.56ms
    HTTP codes:
      1xx - 0, 2xx - 96623, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3377
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3377
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
    Reqs/sec     72791.19    6402.79   91802.63
    Latency      684.85us   195.65us    15.58ms
    HTTP codes:
      1xx - 0, 2xx - 98366, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1634
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1634
    Throughput:    11.32MB/s
  ```


