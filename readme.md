## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `78279` | `5163` | `87767` |
| **90%** | [Hyper Express](#hyper-express) | `70115` | `3202` | `72164` |
| **44%** | [Node (Default)](#node-default) | `34578` | `9710` | `87198` |
| **41%** | [Fastify](#fastify) | `32028` | `9537` | `51964` |
| **40%** | [Hono](#hono) | `31257` | `10929` | `46619` |
| **39%** | [Koa](#koa) | `30410` | `13599` | `76535` |
| **12%** | [Carbon](#carbon) | `9489` | `2460` | `13525` |
| **9%** | [Express](#express) | `7007` | `1617` | `10730` |


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
    Reqs/sec     10657.71    7606.00   82286.32
    Latency        4.69ms     4.32ms   372.72ms
    HTTP codes:
      1xx - 0, 2xx - 90436, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9564
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9564
    Throughput:     2.19MB/s
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
    Reqs/sec      8165.03    6837.29   79702.32
    Latency        6.11ms     3.89ms   356.98ms
    HTTP codes:
      1xx - 0, 2xx - 89876, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10124
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10124
    Throughput:     2.10MB/s
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
    Reqs/sec     31082.17    9954.89   51216.75
    Latency        1.61ms     1.92ms   172.90ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.04MB/s
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
    Reqs/sec     29195.15    8948.56   47553.87
    Latency        1.71ms     2.04ms   179.71ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     6.60MB/s
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
    Reqs/sec     70177.79    3240.71   72531.18
    Latency      710.46us    59.07us     2.94ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.97MB/s
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
    Reqs/sec     29794.98   12981.85   75697.97
    Latency        1.68ms     2.43ms   209.65ms
    HTTP codes:
      1xx - 0, 2xx - 92312, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7688
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7688
    Throughput:     6.21MB/s
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
    Reqs/sec     36674.99   12397.53   84895.80
    Latency        1.36ms     1.82ms   155.69ms
    HTTP codes:
      1xx - 0, 2xx - 94025, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5975
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5975
    Throughput:     7.89MB/s
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
    Reqs/sec     81603.23    2543.95   87154.44
    Latency      610.38us   142.21us     5.84ms
    HTTP codes:
      1xx - 0, 2xx - 96859, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3141
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3141
    Throughput:    12.51MB/s
  ```


