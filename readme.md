## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73917` | `5942` | `85326` |
| **85%** | [Hyper Express](#hyper-express) | `63027` | `4004` | `69399` |
| **30%** | [Node (Default)](#node-default) | `21940` | `4545` | `42065` |
| **29%** | [Fastify](#fastify) | `21248` | `4756` | `35093` |
| **25%** | [Hono](#hono) | `18667` | `3876` | `29397` |
| **23%** | [Koa](#koa) | `16712` | `4349` | `41475` |
| **11%** | [Carbon](#carbon) | `8282` | `1431` | `10635` |
| **9%** | [Express](#express) | `6410` | `1024` | `8492` |


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
    Reqs/sec      8728.96    4102.13   53688.04
    Latency        5.72ms     4.20ms   364.57ms
    HTTP codes:
      1xx - 0, 2xx - 94192, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5808
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5808
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
    Reqs/sec      6316.62     959.96    8448.09
    Latency        7.91ms     3.52ms   337.22ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.81MB/s
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
    Reqs/sec     21117.79    4630.42   34244.70
    Latency        2.36ms     1.91ms   173.02ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.79MB/s
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
    Reqs/sec     17944.98    3445.49   27394.38
    Latency        2.78ms     1.92ms   174.14ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.05MB/s
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
    Reqs/sec     62791.18    3876.57   70656.81
    Latency      794.27us    77.41us     3.36ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.92MB/s
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
    Reqs/sec     16379.47    5143.21   57657.57
    Latency        3.04ms     2.20ms   195.48ms
    HTTP codes:
      1xx - 0, 2xx - 94528, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5472
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5472
    Throughput:     3.50MB/s
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
    Reqs/sec     22734.90    5545.90   59888.22
    Latency        2.19ms     1.77ms   155.39ms
    HTTP codes:
      1xx - 0, 2xx - 96926, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3074
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3074
    Throughput:     5.04MB/s
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
    Reqs/sec     73869.19    5727.69   88266.85
    Latency      674.79us   152.82us     7.68ms
    HTTP codes:
      1xx - 0, 2xx - 96963, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3037
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3037
    Throughput:    11.33MB/s
  ```


