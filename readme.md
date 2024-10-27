## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74013` | `5275` | `84077` |
| **84%** | [Hyper Express](#hyper-express) | `62119` | `3928` | `73048` |
| **37%** | [Node (Default)](#node-default) | `27167` | `8153` | `63424` |
| **32%** | [Fastify](#fastify) | `23666` | `6956` | `35639` |
| **28%** | [Hono](#hono) | `20558` | `5348` | `29316` |
| **26%** | [Koa](#koa) | `19371` | `6144` | `45336` |
| **11%** | [Carbon](#carbon) | `8478` | `1468` | `10653` |
| **9%** | [Express](#express) | `6723` | `1136` | `8634` |


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
    Reqs/sec      9116.30    4777.00   60974.19
    Latency        5.48ms     4.09ms   354.99ms
    HTTP codes:
      1xx - 0, 2xx - 93176, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6824
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6824
    Throughput:     1.93MB/s
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
    Reqs/sec      7151.76    4180.52   56173.33
    Latency        6.98ms     3.79ms   346.83ms
    HTTP codes:
      1xx - 0, 2xx - 92856, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7144
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7136
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 8
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
    Reqs/sec     23927.42    7013.99   36994.53
    Latency        2.09ms     2.03ms   182.22ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.43MB/s
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
    Reqs/sec     20643.11    5829.86   29047.42
    Latency        2.42ms     1.88ms   171.51ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.66MB/s
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
    Reqs/sec     62187.21    3877.60   66760.59
    Latency      801.90us    72.34us     4.36ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.83MB/s
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
    Reqs/sec     18586.96    6894.72   60448.82
    Latency        2.67ms     2.34ms   204.53ms
    HTTP codes:
      1xx - 0, 2xx - 93165, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6835
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6835
    Throughput:     3.93MB/s
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
    Reqs/sec     25336.20    6993.74   51313.09
    Latency        1.97ms     1.77ms   152.45ms
    HTTP codes:
      1xx - 0, 2xx - 97461, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2539
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2539
    Throughput:     5.66MB/s
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
    Reqs/sec     73523.13    5414.83   84940.74
    Latency      679.01us   163.09us    11.48ms
    HTTP codes:
      1xx - 0, 2xx - 97920, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2080
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2080
    Throughput:    11.37MB/s
  ```


