## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75261` | `6545` | `84894` |
| **85%** | [Hyper Express](#hyper-express) | `63611` | `3687` | `67519` |
| **34%** | [Fastify](#fastify) | `25673` | `8219` | `36593` |
| **34%** | [Node (Default)](#node-default) | `25476` | `7270` | `53724` |
| **28%** | [Hono](#hono) | `21050` | `5640` | `29688` |
| **25%** | [Koa](#koa) | `18766` | `6462` | `52193` |
| **11%** | [Carbon](#carbon) | `8334` | `1405` | `10544` |
| **9%** | [Express](#express) | `6500` | `1010` | `8565` |


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
    Reqs/sec      8553.37    4553.93   59814.17
    Latency        5.82ms     4.10ms   357.76ms
    HTTP codes:
      1xx - 0, 2xx - 92715, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7285
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7285
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
    Reqs/sec      6590.58    1063.13    8576.12
    Latency        7.58ms     3.49ms   334.44ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.89MB/s
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
    Reqs/sec     25534.31    8059.34   37198.78
    Latency        1.96ms     1.89ms   172.96ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.78MB/s
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
    Reqs/sec     20891.88    5317.00   30501.06
    Latency        2.39ms     1.87ms   171.06ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.72MB/s
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
    Reqs/sec     63389.13    3670.99   69838.69
    Latency      786.69us    76.22us     3.42ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.00MB/s
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
    Reqs/sec     18313.52    6354.20   50491.87
    Latency        2.72ms     2.32ms   200.46ms
    HTTP codes:
      1xx - 0, 2xx - 94474, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5526
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5526
    Throughput:     3.91MB/s
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
    Reqs/sec     25464.71    7110.21   50204.75
    Latency        1.96ms     1.92ms   164.12ms
    HTTP codes:
      1xx - 0, 2xx - 96970, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3030
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3030
    Throughput:     5.65MB/s
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
    Reqs/sec     74358.73    5837.74   78341.69
    Latency      669.68us   182.60us    14.21ms
    HTTP codes:
      1xx - 0, 2xx - 98184, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1816
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1816
    Throughput:    11.56MB/s
  ```


