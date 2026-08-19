## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `131768` | `5033` | `151908` |
| **91%** | [Hyper Express](#hyper-express) | `119909` | `5369` | `123159` |
| **58%** | [Node (Default)](#node-default) | `76215` | `17362` | `124438` |
| **55%** | [Fastify](#fastify) | `72572` | `17687` | `92153` |
| **49%** | [Hono](#hono) | `64795` | `19991` | `86182` |
| **49%** | [Koa](#koa) | `64092` | `20126` | `140052` |
| **21%** | [Carbon](#carbon) | `28294` | `8033` | `39868` |
| **15%** | [Express](#express) | `19814` | `4145` | `26000` |


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
    Reqs/sec     28770.17   14779.02  127872.70
    Latency        1.73ms     2.80ms   233.85ms
    HTTP codes:
      1xx - 0, 2xx - 91339, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8661
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8661
    Throughput:     5.98MB/s
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
    Reqs/sec     20499.88   12387.05  120968.66
    Latency        2.43ms     2.09ms   183.09ms
    HTTP codes:
      1xx - 0, 2xx - 90951, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9049
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9049
    Throughput:     5.34MB/s
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
    Reqs/sec     81883.41   23605.14  123355.90
    Latency      607.75us     0.95ms    73.08ms
    HTTP codes:
      1xx - 0, 2xx - 81983, 3xx - 0, 4xx - 0, 5xx - 0
      others - 18017
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 18017
    Throughput:    15.25MB/s
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
    Reqs/sec     71113.98   20297.88  109889.27
    Latency      701.19us     0.88ms    70.31ms
    HTTP codes:
      1xx - 0, 2xx - 92312, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7688
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7688
    Throughput:    14.82MB/s
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
    Reqs/sec    118345.16    5855.74  133322.63
    Latency      420.79us   127.35us     5.57ms
    HTTP codes:
      1xx - 0, 2xx - 92476, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7524
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7524
    Throughput:    15.55MB/s
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
    Reqs/sec     64931.20   20357.28  115854.30
    Latency      762.61us     1.16ms    94.03ms
    HTTP codes:
      1xx - 0, 2xx - 92865, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7135
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7135
    Throughput:    13.72MB/s
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
    Reqs/sec     75130.75   15429.09  114568.63
    Latency      662.05us   789.89us    56.72ms
    HTTP codes:
      1xx - 0, 2xx - 96697, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3303
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3303
    Throughput:    16.65MB/s
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
    Reqs/sec    130176.14    3970.51  135418.05
    Latency      382.90us   133.89us     7.39ms
    HTTP codes:
      1xx - 0, 2xx - 96190, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3810
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3810
    Throughput:    19.81MB/s
  ```


