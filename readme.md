## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74301` | `2530` | `77382` |
| **83%** | [Hyper Express](#hyper-express) | `61828` | `3463` | `64117` |
| **33%** | [Node (Default)](#node-default) | `24744` | `7167` | `61885` |
| **32%** | [Fastify](#fastify) | `23840` | `7158` | `35782` |
| **27%** | [Hono](#hono) | `20184` | `5800` | `29312` |
| **24%** | [Koa](#koa) | `17775` | `7484` | `63940` |
| **11%** | [Carbon](#carbon) | `7972` | `1437` | `10195` |
| **8%** | [Express](#express) | `6091` | `961` | `8053` |


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
    Reqs/sec      8990.09    7005.71   77606.93
    Latency        5.54ms     4.39ms   378.72ms
    HTTP codes:
      1xx - 0, 2xx - 88757, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11243
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11243
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
    Reqs/sec      6083.15     995.22    8122.55
    Latency        8.21ms     3.78ms   366.55ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.74MB/s
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
    Reqs/sec     23295.80    7122.75   34723.51
    Latency        2.14ms     1.95ms   174.73ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.29MB/s
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
    Reqs/sec     20023.74    5631.68   28586.73
    Latency        2.49ms     2.02ms   184.03ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.53MB/s
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
    Reqs/sec     61827.50    3651.37   65277.45
    Latency      806.68us    78.13us     3.89ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.78MB/s
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
    Reqs/sec     18354.61    7963.35   65745.70
    Latency        2.71ms     2.43ms   215.88ms
    HTTP codes:
      1xx - 0, 2xx - 92902, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7098
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7098
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
    Reqs/sec     25044.91    7918.25   61519.32
    Latency        1.99ms     2.08ms   178.22ms
    HTTP codes:
      1xx - 0, 2xx - 97465, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2535
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2535
    Throughput:     5.59MB/s
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
    Reqs/sec     74049.58    2826.47   83579.06
    Latency      672.87us   134.59us     6.28ms
    HTTP codes:
      1xx - 0, 2xx - 97215, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2785
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2785
    Throughput:    11.39MB/s
  ```


