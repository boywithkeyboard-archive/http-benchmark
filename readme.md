## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `79362` | `3105` | `86610` |
| **86%** | [Hyper Express](#hyper-express) | `68356` | `3121` | `73301` |
| **43%** | [Node (Default)](#node-default) | `33802` | `10821` | `86778` |
| **41%** | [Fastify](#fastify) | `32899` | `10476` | `52114` |
| **37%** | [Hono](#hono) | `29253` | `8352` | `45845` |
| **36%** | [Koa](#koa) | `28408` | `13212` | `80814` |
| **12%** | [Carbon](#carbon) | `9864` | `2427` | `13331` |
| **9%** | [Express](#express) | `7385` | `1681` | `10408` |


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
    Reqs/sec     10640.62    9027.71   89404.49
    Latency        4.69ms     4.27ms   366.36ms
    HTTP codes:
      1xx - 0, 2xx - 87604, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12396
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12396
    Throughput:     2.12MB/s
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
    Reqs/sec      8424.97    7922.44   86422.48
    Latency        5.92ms     3.73ms   340.02ms
    HTTP codes:
      1xx - 0, 2xx - 87595, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12405
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12405
    Throughput:     2.11MB/s
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
    Reqs/sec     31831.04    9035.58   51578.73
    Latency        1.57ms     1.85ms   167.78ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.22MB/s
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
    Reqs/sec     28890.17    7951.42   46372.89
    Latency        1.73ms     2.05ms   179.79ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     6.53MB/s
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
    Reqs/sec     68964.84    3313.53   72358.75
    Latency      723.13us    79.57us     3.39ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.80MB/s
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
    Reqs/sec     30562.63   12787.36   76961.34
    Latency        1.63ms     2.25ms   192.20ms
    HTTP codes:
      1xx - 0, 2xx - 92396, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7604
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7604
    Throughput:     6.40MB/s
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
    Reqs/sec     34610.35    9480.28   72776.25
    Latency        1.44ms     1.71ms   146.88ms
    HTTP codes:
      1xx - 0, 2xx - 97002, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2998
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2998
    Throughput:     7.68MB/s
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
    Reqs/sec     78929.70    2586.60   83696.22
    Latency      630.62us   143.26us     6.72ms
    HTTP codes:
      1xx - 0, 2xx - 96841, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3159
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3159
    Throughput:    12.10MB/s
  ```


