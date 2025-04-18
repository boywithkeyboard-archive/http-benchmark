## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74087` | `8173` | `89576` |
| **84%** | [Hyper Express](#hyper-express) | `62573` | `3868` | `70067` |
| **35%** | [Node (Default)](#node-default) | `25898` | `7836` | `50899` |
| **33%** | [Fastify](#fastify) | `24654` | `7289` | `36749` |
| **27%** | [Hono](#hono) | `20062` | `5462` | `30408` |
| **26%** | [Koa](#koa) | `19575` | `7932` | `65773` |
| **11%** | [Carbon](#carbon) | `8193` | `1352` | `10357` |
| **9%** | [Express](#express) | `6587` | `1105` | `8491` |


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
    Reqs/sec      8939.71    5173.21   57774.25
    Latency        5.57ms     4.27ms   367.15ms
    HTTP codes:
      1xx - 0, 2xx - 91847, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8153
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8153
    Throughput:     1.87MB/s
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
    Reqs/sec      6888.90    4190.64   47931.51
    Latency        7.25ms     3.91ms   361.42ms
    HTTP codes:
      1xx - 0, 2xx - 92408, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7592
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7592
    Throughput:     1.82MB/s
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
    Reqs/sec     25656.69    7961.95   37445.56
    Latency        1.95ms     1.91ms   173.50ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.82MB/s
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
    Reqs/sec     21560.59    5891.08   30261.54
    Latency        2.32ms     2.10ms   185.79ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.87MB/s
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
    Reqs/sec     63285.88    3978.32   72016.88
    Latency      789.42us    68.48us     3.16ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.97MB/s
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
    Reqs/sec     19064.63    7314.52   60800.87
    Latency        2.62ms     2.43ms   214.17ms
    HTTP codes:
      1xx - 0, 2xx - 92953, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7047
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7047
    Throughput:     4.00MB/s
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
    Reqs/sec     26869.63    7445.02   49414.98
    Latency        1.86ms     1.93ms   164.51ms
    HTTP codes:
      1xx - 0, 2xx - 97884, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2116
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2116
    Throughput:     6.01MB/s
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
    Reqs/sec     74306.92    5580.21   83998.97
    Latency      670.33us   161.02us     6.26ms
    HTTP codes:
      1xx - 0, 2xx - 97631, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2369
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2369
    Throughput:    11.47MB/s
  ```


