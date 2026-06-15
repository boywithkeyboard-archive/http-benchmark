## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `70707` | `4242` | `84174` |
| **80%** | [Hyper Express](#hyper-express) | `56865` | `4459` | `66612` |
| **30%** | [Fastify](#fastify) | `21110` | `5789` | `37160` |
| **29%** | [Hono](#hono) | `20224` | `6189` | `30494` |
| **28%** | [Node (Default)](#node-default) | `19800` | `6183` | `74845` |
| **27%** | [Koa](#koa) | `19195` | `8767` | `63875` |
| **10%** | [Carbon](#carbon) | `7187` | `1169` | `10411` |
| **9%** | [Express](#express) | `6113` | `1118` | `8176` |


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
    Reqs/sec      7836.82    5411.19   63902.06
    Latency        6.36ms     4.55ms   388.75ms
    HTTP codes:
      1xx - 0, 2xx - 91709, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8291
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8291
    Throughput:     1.63MB/s
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
    Reqs/sec      6069.03    1074.54    8283.07
    Latency        8.24ms     3.91ms   372.04ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.74MB/s
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
    Reqs/sec     20895.64    6248.76   36395.87
    Latency        2.39ms     2.06ms   183.21ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.74MB/s
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
    Reqs/sec     20979.85    6275.11   29961.32
    Latency        2.38ms     2.03ms   182.20ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.74MB/s
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
    Reqs/sec     58815.03    3661.28   66220.47
    Latency      847.76us    90.25us     3.05ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.35MB/s
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
    Reqs/sec     19436.58    8617.37   76971.08
    Latency        2.57ms     2.39ms   210.69ms
    HTTP codes:
      1xx - 0, 2xx - 92331, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7669
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7669
    Throughput:     4.05MB/s
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
    Reqs/sec     20020.47    7300.69   77993.60
    Latency        2.49ms     2.06ms   177.55ms
    HTTP codes:
      1xx - 0, 2xx - 94264, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5736
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5736
    Throughput:     4.32MB/s
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
    Reqs/sec     71162.44    4008.71   80309.50
    Latency      699.21us   177.80us    10.35ms
    HTTP codes:
      1xx - 0, 2xx - 96368, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3632
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3632
    Throughput:    10.86MB/s
  ```


