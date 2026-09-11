## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `155580` | `8160` | `163687` |
| **85%** | [Hyper Express](#hyper-express) | `132446` | `9430` | `142228` |
| **35%** | [Node (Default)](#node-default) | `54134` | `13244` | `132846` |
| **32%** | [Fastify](#fastify) | `49709` | `9647` | `70073` |
| **28%** | [Hono](#hono) | `43922` | `8561` | `53014` |
| **27%** | [Koa](#koa) | `42353` | `18185` | `140415` |
| **12%** | [Carbon](#carbon) | `18687` | `5082` | `29516` |
| **9%** | [Express](#express) | `13994` | `2893` | `19339` |


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
    Reqs/sec     21913.03   15211.78  137335.78
    Latency        2.27ms     3.38ms   282.61ms
    HTTP codes:
      1xx - 0, 2xx - 88663, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11337
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11337
    Throughput:     4.42MB/s
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
    Reqs/sec     15961.10   15137.73  141418.39
    Latency        3.12ms     2.47ms   220.21ms
    HTTP codes:
      1xx - 0, 2xx - 85565, 3xx - 0, 4xx - 0, 5xx - 0
      others - 14435
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 14435
    Throughput:     3.92MB/s
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
    Reqs/sec     59066.35   26034.42  141951.62
    Latency      842.39us     1.13ms    85.58ms
    HTTP codes:
      1xx - 0, 2xx - 75351, 3xx - 0, 4xx - 0, 5xx - 0
      others - 24649
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 24649
    Throughput:    10.11MB/s
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
    Reqs/sec     48254.11   23979.97  152534.31
    Latency        1.03ms     1.38ms   111.65ms
    HTTP codes:
      1xx - 0, 2xx - 85398, 3xx - 0, 4xx - 0, 5xx - 0
      others - 14602
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 14602
    Throughput:     9.33MB/s
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
    Reqs/sec    133563.51    7930.56  139714.10
    Latency      372.13us   214.94us    10.27ms
    HTTP codes:
      1xx - 0, 2xx - 90599, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9401
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9401
    Throughput:    17.18MB/s
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
    Reqs/sec     41415.05   20394.74  142417.29
    Latency        1.20ms     1.14ms    98.15ms
    HTTP codes:
      1xx - 0, 2xx - 87335, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12665
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12665
    Throughput:     8.19MB/s
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
    Reqs/sec     55228.03   16984.52  143230.89
    Latency        0.90ms     0.90ms    68.65ms
    HTTP codes:
      1xx - 0, 2xx - 94727, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5273
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5273
    Throughput:    11.99MB/s
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
    Reqs/sec    155906.09    8013.76  162644.04
    Latency      317.88us   130.42us     6.89ms
    HTTP codes:
      1xx - 0, 2xx - 95383, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4617
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4617
    Throughput:    23.52MB/s
  ```


