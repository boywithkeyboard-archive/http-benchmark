## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74837` | `2728` | `84892` |
| **84%** | [Hyper Express](#hyper-express) | `63138` | `3312` | `66066` |
| **35%** | [Node (Default)](#node-default) | `26456` | `8973` | `80508` |
| **33%** | [Fastify](#fastify) | `24527` | `8109` | `36314` |
| **27%** | [Hono](#hono) | `20116` | `5168` | `29386` |
| **26%** | [Koa](#koa) | `19679` | `9036` | `80568` |
| **11%** | [Carbon](#carbon) | `8353` | `1454` | `10400` |
| **9%** | [Express](#express) | `6509` | `1076` | `8514` |


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
    Reqs/sec      8852.40    6044.03   64221.07
    Latency        5.63ms     4.25ms   365.82ms
    HTTP codes:
      1xx - 0, 2xx - 89993, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10007
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10007
    Throughput:     1.81MB/s
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
    Reqs/sec      7067.29    5656.62   72464.26
    Latency        7.07ms     3.89ms   359.23ms
    HTTP codes:
      1xx - 0, 2xx - 90977, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9023
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9023
    Throughput:     1.84MB/s
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
    Reqs/sec     24011.76    7669.07   36801.39
    Latency        2.08ms     2.08ms   187.14ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.45MB/s
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
    Reqs/sec     21153.88    6005.97   29805.95
    Latency        2.36ms     2.09ms   185.05ms
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
    Reqs/sec     63393.27    3955.95   66040.32
    Latency      786.49us    76.04us     3.00ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.01MB/s
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
    Reqs/sec     18663.14    8180.22   67752.14
    Latency        2.68ms     2.41ms   210.74ms
    HTTP codes:
      1xx - 0, 2xx - 93031, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6969
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6969
    Throughput:     3.92MB/s
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
    Reqs/sec     25115.64    7518.45   61455.08
    Latency        1.98ms     1.92ms   164.06ms
    HTTP codes:
      1xx - 0, 2xx - 97508, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2492
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2492
    Throughput:     5.61MB/s
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
    Reqs/sec     74873.50    2412.74   80488.96
    Latency      665.50us   140.88us     7.78ms
    HTTP codes:
      1xx - 0, 2xx - 97066, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2934
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2934
    Throughput:    11.50MB/s
  ```


