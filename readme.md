## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74106` | `5410` | `85401` |
| **85%** | [Hyper Express](#hyper-express) | `63014` | `3236` | `68876` |
| **35%** | [Node (Default)](#node-default) | `26019` | `7449` | `48653` |
| **33%** | [Fastify](#fastify) | `24638` | `6951` | `36522` |
| **28%** | [Hono](#hono) | `20986` | `6010` | `30477` |
| **25%** | [Koa](#koa) | `18455` | `6623` | `61933` |
| **11%** | [Carbon](#carbon) | `8226` | `1423` | `10566` |
| **9%** | [Express](#express) | `6504` | `1030` | `8435` |


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
    Reqs/sec      8782.23    4418.01   53446.23
    Latency        5.68ms     4.27ms   370.45ms
    HTTP codes:
      1xx - 0, 2xx - 93533, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6467
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6467
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
    Reqs/sec      6443.34    1005.05    8500.49
    Latency        7.76ms     3.60ms   342.93ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.84MB/s
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
    Reqs/sec     24049.06    7370.07   35937.19
    Latency        2.08ms     1.97ms   181.05ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.45MB/s
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
    Reqs/sec     20940.63    5636.67   30276.96
    Latency        2.39ms     2.04ms   181.52ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.73MB/s
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
    Reqs/sec     63087.14    3837.81   68334.28
    Latency      788.49us    66.76us     3.48ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.98MB/s
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
    Reqs/sec     17656.32    5836.17   51113.68
    Latency        2.83ms     2.42ms   213.12ms
    HTTP codes:
      1xx - 0, 2xx - 94694, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5306
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5306
    Throughput:     3.78MB/s
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
    Reqs/sec     26036.54    8155.93   65181.61
    Latency        1.92ms     1.94ms   166.98ms
    HTTP codes:
      1xx - 0, 2xx - 97271, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2729
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2729
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
    Reqs/sec     73754.83    5828.56   84751.12
    Latency      674.01us   162.37us    14.43ms
    HTTP codes:
      1xx - 0, 2xx - 97874, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2126
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2126
    Throughput:    11.42MB/s
  ```


