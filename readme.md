## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74538` | `5809` | `85133` |
| **86%** | [Hyper Express](#hyper-express) | `64072` | `2820` | `67150` |
| **36%** | [Node (Default)](#node-default) | `26604` | `8304` | `43548` |
| **33%** | [Fastify](#fastify) | `24801` | `7251` | `36389` |
| **28%** | [Hono](#hono) | `20673` | `5686` | `29213` |
| **25%** | [Koa](#koa) | `18935` | `6916` | `52418` |
| **11%** | [Carbon](#carbon) | `8343` | `1420` | `10651` |
| **9%** | [Express](#express) | `6693` | `1078` | `8626` |


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
    Reqs/sec      8694.92    3684.94   47866.65
    Latency        5.74ms     4.20ms   363.18ms
    HTTP codes:
      1xx - 0, 2xx - 94657, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5343
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5343
    Throughput:     1.87MB/s
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
    Reqs/sec      7027.01    3972.12   49121.70
    Latency        7.09ms     3.78ms   342.55ms
    HTTP codes:
      1xx - 0, 2xx - 92698, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7302
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7298
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 4
    Throughput:     1.87MB/s
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
    Reqs/sec     24551.28    7433.74   36391.61
    Latency        2.03ms     1.98ms   180.26ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.57MB/s
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
    Reqs/sec     20936.43    5343.21   29994.54
    Latency        2.39ms     1.93ms   175.34ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.73MB/s
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
    Reqs/sec     64313.15    4021.11   68294.02
    Latency      775.27us    68.58us     3.81ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.14MB/s
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
    Reqs/sec     18617.54    6682.67   52072.82
    Latency        2.68ms     2.20ms   188.71ms
    HTTP codes:
      1xx - 0, 2xx - 94313, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5687
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5687
    Throughput:     3.97MB/s
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
    Reqs/sec     25815.28    6899.97   50045.63
    Latency        1.93ms     1.88ms   162.70ms
    HTTP codes:
      1xx - 0, 2xx - 97689, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2311
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2311
    Throughput:     5.77MB/s
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
    Reqs/sec     74043.52    6050.42   87355.25
    Latency      671.68us   162.59us    15.22ms
    HTTP codes:
      1xx - 0, 2xx - 98296, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1704
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1704
    Throughput:    11.52MB/s
  ```


