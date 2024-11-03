## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73149` | `6148` | `79471` |
| **87%** | [Hyper Express](#hyper-express) | `63347` | `3482` | `66560` |
| **35%** | [Node (Default)](#node-default) | `25890` | `6727` | `42286` |
| **34%** | [Fastify](#fastify) | `24608` | `7318` | `36301` |
| **29%** | [Hono](#hono) | `20980` | `5708` | `29971` |
| **24%** | [Koa](#koa) | `17730` | `5641` | `43269` |
| **11%** | [Carbon](#carbon) | `8210` | `1371` | `10435` |
| **9%** | [Express](#express) | `6602` | `1046` | `8634` |


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
    Reqs/sec      8967.55    4052.67   53040.97
    Latency        5.57ms     4.26ms   368.03ms
    HTTP codes:
      1xx - 0, 2xx - 94306, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5694
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5694
    Throughput:     1.92MB/s
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
    Reqs/sec      6670.58    1074.84    8651.20
    Latency        7.49ms     3.50ms   333.18ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.91MB/s
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
    Reqs/sec     25522.80    7812.26   36520.00
    Latency        1.96ms     2.07ms   184.94ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.79MB/s
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
    Reqs/sec     20683.53    5509.64   29370.18
    Latency        2.42ms     1.98ms   179.10ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.67MB/s
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
    Reqs/sec     63554.88    4209.04   89010.30
    Latency      788.32us    64.51us     3.46ms
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
    Reqs/sec     18839.55    6107.74   48462.77
    Latency        2.65ms     2.31ms   200.49ms
    HTTP codes:
      1xx - 0, 2xx - 94927, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5073
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5073
    Throughput:     4.05MB/s
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
    Reqs/sec     25527.88    7434.46   56589.36
    Latency        1.96ms     1.78ms   155.57ms
    HTTP codes:
      1xx - 0, 2xx - 97425, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2575
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2575
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
    Reqs/sec     73447.31    5917.29   86825.53
    Latency      678.48us   204.98us    11.63ms
    HTTP codes:
      1xx - 0, 2xx - 95584, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4416
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4416
    Throughput:    11.09MB/s
  ```


