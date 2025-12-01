## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74156` | `2871` | `82465` |
| **83%** | [Hyper Express](#hyper-express) | `61686` | `3637` | `65797` |
| **35%** | [Node (Default)](#node-default) | `26197` | `8368` | `59080` |
| **32%** | [Fastify](#fastify) | `23703` | `7257` | `35763` |
| **28%** | [Hono](#hono) | `21026` | `6084` | `30786` |
| **26%** | [Koa](#koa) | `19587` | `9986` | `81139` |
| **11%** | [Carbon](#carbon) | `8201` | `1453` | `10379` |
| **8%** | [Express](#express) | `6293` | `1021` | `8246` |


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
    Reqs/sec      8860.13    7057.52   74585.66
    Latency        5.63ms     4.29ms   377.21ms
    HTTP codes:
      1xx - 0, 2xx - 89228, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10772
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10772
    Throughput:     1.80MB/s
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
    Reqs/sec      6756.07    4956.54   64646.10
    Latency        7.39ms     3.91ms   358.70ms
    HTTP codes:
      1xx - 0, 2xx - 91666, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8334
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8334
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
    Reqs/sec     24074.82    7667.44   37391.40
    Latency        2.08ms     2.06ms   183.42ms
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
    Reqs/sec     21149.59    5978.68   29730.29
    Latency        2.36ms     2.12ms   186.66ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.78MB/s
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
    Reqs/sec     61363.72    2813.90   65873.23
    Latency      812.93us    66.02us     3.63ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.71MB/s
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
    Reqs/sec     19152.10    8912.87   75090.89
    Latency        2.60ms     2.35ms   206.02ms
    HTTP codes:
      1xx - 0, 2xx - 91601, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8399
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8399
    Throughput:     3.97MB/s
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
    Reqs/sec     24757.19    7490.21   63169.97
    Latency        2.01ms     1.86ms   163.42ms
    HTTP codes:
      1xx - 0, 2xx - 97479, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2521
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2521
    Throughput:     5.53MB/s
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
    Reqs/sec     75130.64    3380.45   92324.82
    Latency      662.30us   190.71us    10.71ms
    HTTP codes:
      1xx - 0, 2xx - 96427, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3573
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3573
    Throughput:    11.47MB/s
  ```


