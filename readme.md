## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73836` | `6555` | `87878` |
| **83%** | [Hyper Express](#hyper-express) | `61380` | `3604` | `64328` |
| **36%** | [Node (Default)](#node-default) | `26417` | `7651` | `38246` |
| **32%** | [Fastify](#fastify) | `23980` | `7178` | `35049` |
| **28%** | [Hono](#hono) | `20563` | `6006` | `28992` |
| **24%** | [Koa](#koa) | `18040` | `6734` | `52441` |
| **11%** | [Carbon](#carbon) | `8283` | `1409` | `10397` |
| **9%** | [Express](#express) | `6514` | `1057` | `8472` |


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
    Reqs/sec      8664.25    3417.62   43871.21
    Latency        5.76ms     4.14ms   355.67ms
    HTTP codes:
      1xx - 0, 2xx - 95354, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4646
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4646
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
    Reqs/sec      6515.33    1026.94    9035.57
    Latency        7.67ms     3.50ms   332.97ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.86MB/s
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
    Reqs/sec     23502.26    7035.78   35836.12
    Latency        2.13ms     2.07ms   186.61ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.33MB/s
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
    Reqs/sec     21127.70    5942.29   30311.49
    Latency        2.36ms     1.91ms   173.71ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.78MB/s
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
    Reqs/sec     62299.49    4242.44   71795.67
    Latency      800.27us    88.83us     4.00ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.85MB/s
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
    Reqs/sec     18246.14    6954.68   61451.05
    Latency        2.73ms     2.36ms   203.99ms
    HTTP codes:
      1xx - 0, 2xx - 93395, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6605
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6605
    Throughput:     3.85MB/s
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
    Reqs/sec     26096.96    7827.41   56810.57
    Latency        1.91ms     1.85ms   158.68ms
    HTTP codes:
      1xx - 0, 2xx - 96929, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3071
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3071
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
    Reqs/sec     74484.72    5127.23   80770.58
    Latency      668.61us   192.49us    13.35ms
    HTTP codes:
      1xx - 0, 2xx - 97227, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2773
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2773
    Throughput:    11.45MB/s
  ```


