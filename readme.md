## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `125079` | `4445` | `133519` |
| **94%** | [Hyper Express](#hyper-express) | `117442` | `5330` | `120018` |
| **61%** | [Node (Default)](#node-default) | `76836` | `20152` | `134266` |
| **60%** | [Fastify](#fastify) | `74974` | `18696` | `89886` |
| **52%** | [Hono](#hono) | `64878` | `16715` | `85772` |
| **50%** | [Koa](#koa) | `62639` | `19057` | `123485` |
| **22%** | [Carbon](#carbon) | `27319` | `7938` | `39100` |
| **15%** | [Express](#express) | `18343` | `4128` | `25164` |


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
    Reqs/sec     27451.82   13794.70  115101.11
    Latency        1.81ms     2.82ms   230.94ms
    HTTP codes:
      1xx - 0, 2xx - 92202, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7798
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7798
    Throughput:     5.76MB/s
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
    Reqs/sec     20219.49   12462.17  123391.01
    Latency        2.47ms     2.20ms   192.76ms
    HTTP codes:
      1xx - 0, 2xx - 90147, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9853
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9853
    Throughput:     5.22MB/s
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
    Reqs/sec     75153.71   21199.05  109641.43
    Latency      663.28us     1.07ms    86.87ms
    HTTP codes:
      1xx - 0, 2xx - 86225, 3xx - 0, 4xx - 0, 5xx - 0
      others - 13775
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 13775
    Throughput:    14.71MB/s
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
    Reqs/sec     70349.93   20104.83  127094.16
    Latency      709.27us   769.77us    51.89ms
    HTTP codes:
      1xx - 0, 2xx - 91924, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8076
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8076
    Throughput:    14.59MB/s
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
    Reqs/sec    118849.13    4770.22  131128.03
    Latency      419.11us   133.81us     5.16ms
    HTTP codes:
      1xx - 0, 2xx - 91703, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8297
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8297
    Throughput:    15.47MB/s
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
    Reqs/sec     65863.25   20571.03  137035.22
    Latency      755.78us     1.18ms    95.23ms
    HTTP codes:
      1xx - 0, 2xx - 91049, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8951
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8951
    Throughput:    13.56MB/s
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
    Reqs/sec     78442.46   19390.31  139342.19
    Latency      635.34us   595.99us    50.72ms
    HTTP codes:
      1xx - 0, 2xx - 93603, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6397
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6397
    Throughput:    16.81MB/s
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
    Reqs/sec    124443.15    4482.26  130242.85
    Latency      399.77us    86.61us     4.93ms
    HTTP codes:
      1xx - 0, 2xx - 97281, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2719
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2719
    Throughput:    19.18MB/s
  ```


