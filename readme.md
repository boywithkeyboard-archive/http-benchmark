## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72350` | `5480` | `75664` |
| **86%** | [Hyper Express](#hyper-express) | `62079` | `4404` | `70725` |
| **35%** | [Node (Default)](#node-default) | `25116` | `7185` | `47470` |
| **33%** | [Fastify](#fastify) | `23856` | `7034` | `35205` |
| **29%** | [Hono](#hono) | `21161` | `5937` | `29202` |
| **25%** | [Koa](#koa) | `18132` | `5706` | `43521` |
| **11%** | [Carbon](#carbon) | `8192` | `1415` | `10427` |
| **9%** | [Express](#express) | `6478` | `1069` | `8387` |


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
    Reqs/sec      8679.71    3919.85   51454.97
    Latency        5.75ms     4.19ms   363.87ms
    HTTP codes:
      1xx - 0, 2xx - 94713, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5287
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5287
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
    Reqs/sec      6520.97    1029.72    8435.79
    Latency        7.66ms     3.62ms   346.30ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.87MB/s
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
    Reqs/sec     24071.34    7103.22   35560.69
    Latency        2.08ms     1.99ms   179.62ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.46MB/s
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
    Reqs/sec     20737.19    5526.95   30024.40
    Latency        2.41ms     1.94ms   174.67ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.69MB/s
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
    Reqs/sec     62630.88    2883.14   68838.54
    Latency      796.05us    76.28us     3.71ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.90MB/s
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
    Reqs/sec     17476.54    5769.67   46203.21
    Latency        2.85ms     2.32ms   201.15ms
    HTTP codes:
      1xx - 0, 2xx - 95427, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4573
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4573
    Throughput:     3.77MB/s
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
    Reqs/sec     25879.12    7967.21   52581.04
    Latency        1.93ms     1.83ms   156.21ms
    HTTP codes:
      1xx - 0, 2xx - 97839, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2161
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2161
    Throughput:     5.80MB/s
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
    Reqs/sec     73596.98    5760.09   89012.41
    Latency      675.93us   222.01us    15.35ms
    HTTP codes:
      1xx - 0, 2xx - 96196, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3804
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3804
    Throughput:    11.21MB/s
  ```


