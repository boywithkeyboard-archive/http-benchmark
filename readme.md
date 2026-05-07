## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `70054` | `4427` | `84357` |
| **84%** | [Hyper Express](#hyper-express) | `58604` | `3929` | `67059` |
| **29%** | [Hono](#hono) | `20536` | `6242` | `29211` |
| **29%** | [Node (Default)](#node-default) | `20236` | `5776` | `57885` |
| **29%** | [Fastify](#fastify) | `20106` | `5236` | `35812` |
| **26%** | [Koa](#koa) | `18155` | `8497` | `65016` |
| **10%** | [Carbon](#carbon) | `7280` | `1187` | `10158` |
| **9%** | [Express](#express) | `5982` | `1048` | `8129` |


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
    Reqs/sec      7841.52    5833.24   68694.81
    Latency        6.37ms     4.59ms   395.28ms
    HTTP codes:
      1xx - 0, 2xx - 90676, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9324
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9324
    Throughput:     1.61MB/s
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
    Reqs/sec      6181.39    1120.34    8179.70
    Latency        8.09ms     3.98ms   378.33ms
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
    Reqs/sec     21280.92    6786.37   36689.33
    Latency        2.35ms     2.14ms   193.59ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.83MB/s
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
    Reqs/sec     20898.59    6199.75   29377.45
    Latency        2.39ms     2.15ms   195.31ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.72MB/s
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
    Reqs/sec     58231.10    3570.08   63365.30
    Latency        0.86ms    91.73us     5.11ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.27MB/s
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
    Reqs/sec     19233.07   10571.46   79365.28
    Latency        2.59ms     2.64ms   229.92ms
    HTTP codes:
      1xx - 0, 2xx - 88256, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11744
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11744
    Throughput:     3.84MB/s
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
    Reqs/sec     20330.42    5673.76   64008.89
    Latency        2.45ms     2.14ms   185.31ms
    HTTP codes:
      1xx - 0, 2xx - 97150, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2850
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2850
    Throughput:     4.52MB/s
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
    Reqs/sec     71787.60    5566.94   93284.87
    Latency      692.92us   206.80us    13.46ms
    HTTP codes:
      1xx - 0, 2xx - 96692, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3308
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3308
    Throughput:    10.99MB/s
  ```


