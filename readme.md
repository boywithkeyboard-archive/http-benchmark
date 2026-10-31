## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `70451` | `3657` | `79535` |
| **84%** | [Hyper Express](#hyper-express) | `59023` | `3766` | `63910` |
| **36%** | [Node (Default)](#node-default) | `25142` | `9139` | `73836` |
| **35%** | [Fastify](#fastify) | `24339` | `7502` | `34819` |
| **27%** | [Hono](#hono) | `18764` | `5439` | `28371` |
| **25%** | [Koa](#koa) | `17847` | `8112` | `70685` |
| **11%** | [Carbon](#carbon) | `7818` | `1323` | `10334` |
| **9%** | [Express](#express) | `6164` | `986` | `8248` |


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
    Reqs/sec      8294.17    5430.88   73026.38
    Latency        6.02ms     4.74ms   399.89ms
    HTTP codes:
      1xx - 0, 2xx - 92025, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7975
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7975
    Throughput:     1.73MB/s
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
    Reqs/sec      6194.52    1053.14    8098.88
    Latency        8.07ms     3.88ms   373.80ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.77MB/s
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
    Reqs/sec     22565.06    6684.14   32982.05
    Latency        2.22ms     2.08ms   185.98ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.12MB/s
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
    Reqs/sec     18891.47    5245.71   27877.34
    Latency        2.65ms     2.30ms   201.80ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.27MB/s
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
    Reqs/sec     59093.95    3685.30   67130.87
    Latency      843.33us    95.74us     3.98ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.40MB/s
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
    Reqs/sec     17077.75    6634.51   59922.19
    Latency        2.92ms     2.48ms   217.37ms
    HTTP codes:
      1xx - 0, 2xx - 93892, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6108
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6108
    Throughput:     3.63MB/s
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
    Reqs/sec     26023.57    9691.10   74918.16
    Latency        1.92ms     2.02ms   175.10ms
    HTTP codes:
      1xx - 0, 2xx - 94190, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5810
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5810
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
    Reqs/sec     70990.37    5512.95   86180.22
    Latency      697.14us   224.20us    12.83ms
    HTTP codes:
      1xx - 0, 2xx - 96637, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3363
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3363
    Throughput:    10.90MB/s
  ```


