## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73529` | `6029` | `79192` |
| **87%** | [Hyper Express](#hyper-express) | `63946` | `4038` | `79613` |
| **35%** | [Node (Default)](#node-default) | `25494` | `7424` | `46991` |
| **33%** | [Fastify](#fastify) | `24335` | `7066` | `35629` |
| **28%** | [Hono](#hono) | `20820` | `5629` | `29389` |
| **24%** | [Koa](#koa) | `17871` | `6487` | `48336` |
| **11%** | [Carbon](#carbon) | `8157` | `1386` | `10387` |
| **9%** | [Express](#express) | `6389` | `963` | `8530` |


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
    Reqs/sec      8467.80    3727.15   52354.76
    Latency        5.90ms     4.17ms   363.77ms
    HTTP codes:
      1xx - 0, 2xx - 94937, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5063
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5063
    Throughput:     1.82MB/s
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
    Reqs/sec      6607.34    1084.07    8627.38
    Latency        7.56ms     3.41ms   331.76ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.89MB/s
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
    Reqs/sec     24515.07    7415.65   35497.95
    Latency        2.04ms     1.93ms   173.80ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.55MB/s
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
    Reqs/sec     21018.92    5584.13   30062.93
    Latency        2.38ms     1.93ms   174.40ms
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
    Reqs/sec     63459.14    4695.78   79563.18
    Latency      785.51us    70.09us     3.22ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.01MB/s
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
    Reqs/sec     17629.17    5718.01   47210.61
    Latency        2.83ms     2.40ms   208.25ms
    HTTP codes:
      1xx - 0, 2xx - 95185, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4815
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4815
    Throughput:     3.79MB/s
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
    Reqs/sec     24899.04    7284.51   45515.98
    Latency        2.00ms     1.92ms   162.37ms
    HTTP codes:
      1xx - 0, 2xx - 97885, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2115
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2115
    Throughput:     5.60MB/s
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
    Reqs/sec     74413.12    6232.02   81349.90
    Latency      669.99us   240.44us    18.82ms
    HTTP codes:
      1xx - 0, 2xx - 98282, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1718
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1718
    Throughput:    11.57MB/s
  ```


