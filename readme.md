## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `78288` | `2507` | `81625` |
| **87%** | [Hyper Express](#hyper-express) | `68330` | `3767` | `71816` |
| **49%** | [Node (Default)](#node-default) | `38128` | `11495` | `75943` |
| **45%** | [Fastify](#fastify) | `35229` | `11211` | `52251` |
| **38%** | [Koa](#koa) | `29793` | `12864` | `78137` |
| **37%** | [Hono](#hono) | `29009` | `8430` | `47742` |
| **12%** | [Carbon](#carbon) | `9661` | `2497` | `13955` |
| **10%** | [Express](#express) | `7946` | `1839` | `10788` |


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
    Reqs/sec     10401.90    6569.86   78959.38
    Latency        4.79ms     4.24ms   365.39ms
    HTTP codes:
      1xx - 0, 2xx - 92071, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7929
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7929
    Throughput:     2.18MB/s
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
    Reqs/sec      8305.32    7993.19   83583.21
    Latency        6.01ms     3.73ms   340.72ms
    HTTP codes:
      1xx - 0, 2xx - 87088, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12912
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12912
    Throughput:     2.07MB/s
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
    Reqs/sec     34812.88   10286.96   52021.97
    Latency        1.43ms     1.94ms   170.96ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.90MB/s
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
    Reqs/sec     30820.79    8870.46   48520.17
    Latency        1.62ms     1.69ms   155.24ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     6.96MB/s
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
    Reqs/sec     69102.44    3632.15   73807.30
    Latency      721.89us    77.68us     2.65ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.81MB/s
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
    Reqs/sec     31994.52   13482.98   86810.84
    Latency        1.56ms     2.19ms   191.10ms
    HTTP codes:
      1xx - 0, 2xx - 90609, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9391
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9391
    Throughput:     6.56MB/s
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
    Reqs/sec     37773.87   11433.15   77942.26
    Latency        1.32ms     1.79ms   154.44ms
    HTTP codes:
      1xx - 0, 2xx - 96620, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3380
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3380
    Throughput:     8.36MB/s
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
    Reqs/sec     78409.06    3201.84   87735.06
    Latency      635.68us   155.07us     8.95ms
    HTTP codes:
      1xx - 0, 2xx - 97247, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2753
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2753
    Throughput:    12.07MB/s
  ```


