## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75227` | `2646` | `79472` |
| **84%** | [Hyper Express](#hyper-express) | `62861` | `3759` | `68508` |
| **34%** | [Node (Default)](#node-default) | `25835` | `8507` | `74655` |
| **31%** | [Fastify](#fastify) | `23479` | `6859` | `35529` |
| **27%** | [Hono](#hono) | `20432` | `5927` | `29260` |
| **26%** | [Koa](#koa) | `19231` | `8952` | `75998` |
| **11%** | [Carbon](#carbon) | `8218` | `1426` | `10336` |
| **9%** | [Express](#express) | `6439` | `1064` | `8355` |


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
    Reqs/sec      8778.49    5682.50   77307.03
    Latency        5.68ms     4.31ms   371.35ms
    HTTP codes:
      1xx - 0, 2xx - 92430, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7570
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7570
    Throughput:     1.84MB/s
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
    Reqs/sec      7051.71    5623.02   78490.68
    Latency        7.07ms     3.81ms   354.22ms
    HTTP codes:
      1xx - 0, 2xx - 90979, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9021
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9021
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
    Reqs/sec     24427.25    7792.01   35906.99
    Latency        2.04ms     2.03ms   182.79ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.54MB/s
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
    Reqs/sec     20603.59    5527.45   29402.85
    Latency        2.42ms     2.08ms   185.26ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.66MB/s
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
    Reqs/sec     63353.13    3767.29   67651.50
    Latency      787.07us    68.48us     2.82ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.00MB/s
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
    Reqs/sec     18730.12    8529.10   72155.47
    Latency        2.66ms     2.44ms   215.74ms
    HTTP codes:
      1xx - 0, 2xx - 92124, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7876
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7876
    Throughput:     3.90MB/s
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
    Reqs/sec     25774.25    8481.27   75228.82
    Latency        1.93ms     1.94ms   161.08ms
    HTTP codes:
      1xx - 0, 2xx - 96348, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3652
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3652
    Throughput:     5.69MB/s
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
    Reqs/sec     75722.23    3074.36   87329.59
    Latency      657.84us   141.39us     5.05ms
    HTTP codes:
      1xx - 0, 2xx - 97463, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2537
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2537
    Throughput:    11.68MB/s
  ```


