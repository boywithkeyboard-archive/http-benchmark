## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74700` | `6137` | `85655` |
| **84%** | [Hyper Express](#hyper-express) | `62841` | `3827` | `67974` |
| **37%** | [Node (Default)](#node-default) | `27809` | `8781` | `57849` |
| **32%** | [Fastify](#fastify) | `24262` | `7491` | `36672` |
| **27%** | [Hono](#hono) | `20319` | `5334` | `29123` |
| **25%** | [Koa](#koa) | `18559` | `6900` | `57340` |
| **11%** | [Carbon](#carbon) | `8206` | `1360` | `10291` |
| **9%** | [Express](#express) | `6436` | `1074` | `8452` |


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
    Reqs/sec      8889.75    4288.21   55446.39
    Latency        5.61ms     4.23ms   362.99ms
    HTTP codes:
      1xx - 0, 2xx - 93800, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6200
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6200
    Throughput:     1.89MB/s
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
    Reqs/sec      7094.21    4184.45   55408.41
    Latency        7.04ms     3.89ms   355.41ms
    HTTP codes:
      1xx - 0, 2xx - 92973, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7027
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7027
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
    Reqs/sec     24999.90    7890.14   36905.09
    Latency        2.00ms     2.03ms   181.66ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.67MB/s
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
    Reqs/sec     21495.92    6479.02   30164.39
    Latency        2.32ms     2.05ms   184.22ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.86MB/s
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
    Reqs/sec     62121.46    3553.19   66666.32
    Latency      802.66us    66.25us     4.32ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.82MB/s
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
    Reqs/sec     18794.43    6368.22   53260.93
    Latency        2.65ms     2.40ms   211.21ms
    HTTP codes:
      1xx - 0, 2xx - 94559, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5441
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5441
    Throughput:     4.02MB/s
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
    Reqs/sec     26120.25    7784.08   49137.19
    Latency        1.91ms     1.89ms   160.98ms
    HTTP codes:
      1xx - 0, 2xx - 97786, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2214
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2214
    Throughput:     5.85MB/s
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
    Reqs/sec     74142.09    5216.03   84009.47
    Latency      672.14us   158.39us     6.90ms
    HTTP codes:
      1xx - 0, 2xx - 97737, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2263
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2263
    Throughput:    11.47MB/s
  ```


