## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `68079` | `3936` | `81410` |
| **84%** | [Hyper Express](#hyper-express) | `57109` | `2970` | `62186` |
| **33%** | [Hono](#hono) | `22143` | `7219` | `30658` |
| **31%** | [Fastify](#fastify) | `20860` | `5641` | `35898` |
| **30%** | [Node (Default)](#node-default) | `20336` | `6240` | `75635` |
| **28%** | [Koa](#koa) | `19042` | `8789` | `70294` |
| **11%** | [Carbon](#carbon) | `7320` | `1234` | `10340` |
| **9%** | [Express](#express) | `6196` | `1128` | `8256` |


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
    Reqs/sec      7701.87    4934.98   63186.30
    Latency        6.48ms     4.67ms   395.22ms
    HTTP codes:
      1xx - 0, 2xx - 92660, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7340
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7340
    Throughput:     1.62MB/s
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
    Reqs/sec      6108.22    1082.94    8193.74
    Latency        8.18ms     3.83ms   366.67ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.75MB/s
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
    Reqs/sec     20031.92    5578.00   35695.87
    Latency        2.49ms     2.25ms   200.46ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.55MB/s
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
    Reqs/sec     21321.08    6665.37   30437.69
    Latency        2.34ms     2.42ms   212.90ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.82MB/s
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
    Reqs/sec     57342.32    3067.41   61519.92
    Latency        0.87ms    91.42us     3.01ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.15MB/s
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
    Reqs/sec     19548.88   10956.21   77031.56
    Latency        2.55ms     2.49ms   218.13ms
    HTTP codes:
      1xx - 0, 2xx - 87535, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12465
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12465
    Throughput:     3.88MB/s
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
    Reqs/sec     20374.57    4992.12   58448.52
    Latency        2.45ms     1.99ms   170.02ms
    HTTP codes:
      1xx - 0, 2xx - 97577, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2423
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2423
    Throughput:     4.55MB/s
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
    Reqs/sec     68530.91    4236.30   81099.63
    Latency      726.37us   219.77us    10.23ms
    HTTP codes:
      1xx - 0, 2xx - 96155, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3845
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3845
    Throughput:    10.43MB/s
  ```


