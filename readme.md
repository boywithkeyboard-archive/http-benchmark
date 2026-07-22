## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `78752` | `2032` | `82100` |
| **87%** | [Hyper Express](#hyper-express) | `68541` | `3451` | `72147` |
| **46%** | [Node (Default)](#node-default) | `36186` | `10970` | `81128` |
| **44%** | [Fastify](#fastify) | `34691` | `10954` | `50845` |
| **39%** | [Koa](#koa) | `30451` | `13965` | `90634` |
| **38%** | [Hono](#hono) | `30051` | `8324` | `45975` |
| **12%** | [Carbon](#carbon) | `9444` | `2416` | `13630` |
| **9%** | [Express](#express) | `7348` | `1606` | `10459` |


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
    Reqs/sec     10337.33    7305.97   87877.69
    Latency        4.83ms     4.24ms   364.84ms
    HTTP codes:
      1xx - 0, 2xx - 91026, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8974
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8974
    Throughput:     2.14MB/s
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
    Reqs/sec      8389.69    6889.40   75457.07
    Latency        5.94ms     3.79ms   349.50ms
    HTTP codes:
      1xx - 0, 2xx - 89652, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10348
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10347
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 1
    Throughput:     2.16MB/s
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
    Reqs/sec     32991.41    8859.99   50950.10
    Latency        1.51ms     1.79ms   162.08ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.49MB/s
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
    Reqs/sec     31065.45    9907.36   46504.81
    Latency        1.61ms     2.06ms   187.03ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.00MB/s
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
    Reqs/sec     67824.86    2511.03   69950.77
    Latency      735.31us    70.02us     3.18ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.64MB/s
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
    Reqs/sec     31371.73   12723.10   77485.73
    Latency        1.59ms     2.23ms   192.36ms
    HTTP codes:
      1xx - 0, 2xx - 92474, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7526
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7526
    Throughput:     6.57MB/s
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
    Reqs/sec     35609.78   10005.99   83961.88
    Latency        1.40ms     1.70ms   143.21ms
    HTTP codes:
      1xx - 0, 2xx - 95611, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4389
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4389
    Throughput:     7.78MB/s
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
    Reqs/sec     78083.84    2892.37   82918.24
    Latency      637.51us   169.34us     8.55ms
    HTTP codes:
      1xx - 0, 2xx - 96610, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3390
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3390
    Throughput:    11.94MB/s
  ```


