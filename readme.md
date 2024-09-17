## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73477` | `5799` | `92967` |
| **86%** | [Hyper Express](#hyper-express) | `62887` | `4168` | `69306` |
| **35%** | [Node (Default)](#node-default) | `25396` | `7015` | `49337` |
| **33%** | [Fastify](#fastify) | `24388` | `7113` | `35962` |
| **28%** | [Hono](#hono) | `20759` | `5513` | `29678` |
| **26%** | [Koa](#koa) | `19143` | `6579` | `53067` |
| **11%** | [Carbon](#carbon) | `8312` | `1460` | `10567` |
| **9%** | [Express](#express) | `6494` | `1040` | `8481` |


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
    Reqs/sec      8796.35    4782.01   53405.55
    Latency        5.67ms     4.18ms   360.44ms
    HTTP codes:
      1xx - 0, 2xx - 92263, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7737
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7737
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
    Reqs/sec      6922.72    4614.95   54742.30
    Latency        7.22ms     3.97ms   362.73ms
    HTTP codes:
      1xx - 0, 2xx - 91064, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8936
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8930
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 6
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
    Reqs/sec     23640.58    6937.70   36393.80
    Latency        2.11ms     2.10ms   188.19ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.37MB/s
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
    Reqs/sec     20891.20    6019.98   29743.51
    Latency        2.39ms     2.04ms   183.26ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.72MB/s
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
    Reqs/sec     61444.82    3787.10   64670.89
    Latency      810.45us    68.81us     3.00ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.74MB/s
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
    Reqs/sec     18219.60    6352.31   50055.21
    Latency        2.74ms     2.42ms   210.72ms
    HTTP codes:
      1xx - 0, 2xx - 94569, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5431
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5431
    Throughput:     3.89MB/s
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
    Reqs/sec     25385.22    7817.47   41746.28
    Latency        1.96ms     1.91ms   163.28ms
    HTTP codes:
      1xx - 0, 2xx - 98404, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1596
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1596
    Throughput:     5.72MB/s
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
    Reqs/sec     73698.35    4974.65   78589.85
    Latency      674.57us   201.47us     9.30ms
    HTTP codes:
      1xx - 0, 2xx - 96774, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3226
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3226
    Throughput:    11.29MB/s
  ```


