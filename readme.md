## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `77055` | `5796` | `90702` |
| **81%** | [Hyper Express](#hyper-express) | `62730` | `3788` | `66304` |
| **30%** | [Node (Default)](#node-default) | `23135` | `5258` | `43476` |
| **28%** | [Fastify](#fastify) | `21317` | `4938` | `37026` |
| **24%** | [Hono](#hono) | `18380` | `3652` | `28939` |
| **21%** | [Koa](#koa) | `16468` | `5815` | `53442` |
| **11%** | [Carbon](#carbon) | `8176` | `1412` | `10373` |
| **8%** | [Express](#express) | `6487` | `1110` | `8544` |


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
    Reqs/sec      8910.78    5343.17   60933.13
    Latency        5.60ms     4.41ms   382.16ms
    HTTP codes:
      1xx - 0, 2xx - 91279, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8721
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8721
    Throughput:     1.85MB/s
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
    Reqs/sec      6460.59    1083.76    8519.38
    Latency        7.74ms     3.80ms   361.02ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.85MB/s
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
    Reqs/sec     21658.00    5179.72   36096.92
    Latency        2.31ms     2.10ms   186.53ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.91MB/s
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
    Reqs/sec     19270.12    4641.12   30111.46
    Latency        2.59ms     2.00ms   178.60ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.36MB/s
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
    Reqs/sec     63586.37    4024.09   68444.90
    Latency      784.15us    69.07us     2.79ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.03MB/s
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
    Reqs/sec     17071.41    5752.22   53746.99
    Latency        2.93ms     2.42ms   216.15ms
    HTTP codes:
      1xx - 0, 2xx - 93762, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6238
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6238
    Throughput:     3.61MB/s
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
    Reqs/sec     24383.07    6227.23   44802.87
    Latency        2.05ms     1.91ms   161.99ms
    HTTP codes:
      1xx - 0, 2xx - 98099, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1901
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1901
    Throughput:     5.48MB/s
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
    Reqs/sec     74300.54    4912.74   81550.52
    Latency      670.37us   164.34us     7.34ms
    HTTP codes:
      1xx - 0, 2xx - 96837, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3163
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3163
    Throughput:    11.37MB/s
  ```


