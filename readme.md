## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `71053` | `4635` | `81830` |
| **82%** | [Hyper Express](#hyper-express) | `58433` | `3526` | `73530` |
| **36%** | [Node (Default)](#node-default) | `25445` | `9093` | `69346` |
| **32%** | [Fastify](#fastify) | `22903` | `7346` | `33350` |
| **25%** | [Hono](#hono) | `17734` | `5019` | `28213` |
| **25%** | [Koa](#koa) | `17714` | `8117` | `75658` |
| **11%** | [Carbon](#carbon) | `7597` | `1410` | `10569` |
| **9%** | [Express](#express) | `6087` | `987` | `8240` |


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
    Reqs/sec      8281.83    5236.41   69435.84
    Latency        6.02ms     4.51ms   386.43ms
    HTTP codes:
      1xx - 0, 2xx - 92458, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7542
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7542
    Throughput:     1.74MB/s
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
    Reqs/sec      5964.27     882.55    8164.01
    Latency        8.38ms     3.75ms   360.56ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.71MB/s
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
    Reqs/sec     24145.20    8548.11   35690.62
    Latency        2.07ms     2.25ms   198.83ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.47MB/s
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
    Reqs/sec     18400.91    5427.80   28073.12
    Latency        2.72ms     2.02ms   186.82ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.15MB/s
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
    Reqs/sec     58135.51    3555.32   64149.91
    Latency        0.86ms   120.71us     4.79ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.25MB/s
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
    Reqs/sec     18292.17   10588.76   80111.29
    Latency        2.72ms     2.52ms   217.60ms
    HTTP codes:
      1xx - 0, 2xx - 87795, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12205
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12205
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
    Reqs/sec     25891.22    8975.07   72354.02
    Latency        1.92ms     2.02ms   174.23ms
    HTTP codes:
      1xx - 0, 2xx - 96735, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3265
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3265
    Throughput:     5.74MB/s
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
    Reqs/sec     70460.47    4310.53   83097.66
    Latency      706.88us   165.49us     8.47ms
    HTTP codes:
      1xx - 0, 2xx - 97142, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2858
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2858
    Throughput:    10.83MB/s
  ```


