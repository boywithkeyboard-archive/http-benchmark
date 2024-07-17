## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74607` | `5642` | `84071` |
| **87%** | [Hyper Express](#hyper-express) | `65104` | `3727` | `68078` |
| **35%** | [Node (Default)](#node-default) | `25822` | `7923` | `41392` |
| **34%** | [Fastify](#fastify) | `25399` | `8363` | `36729` |
| **27%** | [Hono](#hono) | `20508` | `5438` | `29962` |
| **25%** | [Koa](#koa) | `18374` | `6253` | `56415` |
| **11%** | [Carbon](#carbon) | `8146` | `1373` | `10192` |
| **9%** | [Express](#express) | `6514` | `1034` | `8460` |


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
    Reqs/sec      8704.94    4461.14   57673.54
    Latency        5.72ms     4.18ms   364.26ms
    HTTP codes:
      1xx - 0, 2xx - 92869, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7131
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7131
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
    Reqs/sec      6533.13    1058.64    8430.63
    Latency        7.65ms     3.67ms   344.88ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     24791.77    8178.89   36990.23
    Latency        2.02ms     2.05ms   183.08ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.62MB/s
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
    Reqs/sec     20590.02    5730.75   29921.43
    Latency        2.43ms     1.95ms   178.28ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.65MB/s
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
    Reqs/sec     64725.20    4126.94   69722.06
    Latency      770.77us    76.59us     3.34ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.19MB/s
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
    Reqs/sec     18257.34    6998.77   63807.91
    Latency        2.73ms     2.43ms   211.14ms
    HTTP codes:
      1xx - 0, 2xx - 92542, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7458
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7458
    Throughput:     3.82MB/s
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
    Reqs/sec     26970.68    8065.52   41269.50
    Latency        1.85ms     1.91ms   163.46ms
    HTTP codes:
      1xx - 0, 2xx - 98428, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1572
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1572
    Throughput:     6.07MB/s
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
    Reqs/sec     75496.43    6276.09   83726.43
    Latency      660.35us   318.86us    17.19ms
    HTTP codes:
      1xx - 0, 2xx - 97516, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2484
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2484
    Throughput:    11.64MB/s
  ```


