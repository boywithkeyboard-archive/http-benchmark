## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `69959` | `4680` | `86271` |
| **83%** | [Hyper Express](#hyper-express) | `58123` | `3555` | `67065` |
| **30%** | [Hono](#hono) | `21047` | `6425` | `29304` |
| **30%** | [Node (Default)](#node-default) | `20844` | `5979` | `69091` |
| **29%** | [Fastify](#fastify) | `19952` | `5257` | `35210` |
| **26%** | [Koa](#koa) | `18082` | `8498` | `76570` |
| **11%** | [Carbon](#carbon) | `7597` | `1469` | `10369` |
| **9%** | [Express](#express) | `6072` | `1095` | `8168` |


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
    Reqs/sec      8052.88    5902.23   73843.19
    Latency        6.20ms     4.57ms   390.53ms
    HTTP codes:
      1xx - 0, 2xx - 90992, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9008
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9008
    Throughput:     1.66MB/s
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
    Reqs/sec      6130.29    1109.27    8236.61
    Latency        8.15ms     4.01ms   379.02ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.75MB/s
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
    Reqs/sec     20773.95    5666.99   34727.85
    Latency        2.40ms     2.14ms   188.05ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.71MB/s
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
    Reqs/sec     20456.32    6339.17   29698.08
    Latency        2.44ms     2.11ms   187.45ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.62MB/s
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
    Reqs/sec     57643.85    3414.20   62225.22
    Latency        0.87ms    98.55us     3.12ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.19MB/s
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
    Reqs/sec     17968.25    8763.78   79180.24
    Latency        2.78ms     2.54ms   222.60ms
    HTTP codes:
      1xx - 0, 2xx - 91253, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8747
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8747
    Throughput:     3.71MB/s
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
    Reqs/sec     19625.99    5346.37   67419.29
    Latency        2.54ms     2.09ms   177.58ms
    HTTP codes:
      1xx - 0, 2xx - 96641, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3359
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3359
    Throughput:     4.35MB/s
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
    Reqs/sec     70514.75    4478.45   85632.82
    Latency      705.85us   174.73us     9.86ms
    HTTP codes:
      1xx - 0, 2xx - 96739, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3261
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3261
    Throughput:    10.80MB/s
  ```


