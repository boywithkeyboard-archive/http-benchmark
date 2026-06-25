## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `71511` | `3646` | `80101` |
| **81%** | [Hyper Express](#hyper-express) | `58171` | `4301` | `66286` |
| **32%** | [Node (Default)](#node-default) | `22743` | `6200` | `67018` |
| **32%** | [Hono](#hono) | `22740` | `6243` | `30117` |
| **30%** | [Fastify](#fastify) | `21619` | `5662` | `37418` |
| **26%** | [Koa](#koa) | `18486` | `9861` | `79150` |
| **11%** | [Carbon](#carbon) | `7752` | `1355` | `10360` |
| **9%** | [Express](#express) | `6339` | `1033` | `8341` |


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
    Reqs/sec      8100.93    5474.10   66996.33
    Latency        6.16ms     4.36ms   372.27ms
    HTTP codes:
      1xx - 0, 2xx - 91598, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8402
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8402
    Throughput:     1.69MB/s
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
    Reqs/sec      6325.01    1036.47    8325.10
    Latency        7.90ms     3.80ms   363.54ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.81MB/s
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
    Reqs/sec     22246.06    5746.33   37049.95
    Latency        2.24ms     1.99ms   179.78ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.05MB/s
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
    Reqs/sec     20408.99    5589.20   30035.33
    Latency        2.45ms     2.02ms   181.10ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.61MB/s
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
    Reqs/sec     59981.20    3797.67   66817.15
    Latency      831.42us    92.09us     2.99ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.52MB/s
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
    Reqs/sec     18437.38    8327.23   68634.55
    Latency        2.71ms     2.45ms   210.87ms
    HTTP codes:
      1xx - 0, 2xx - 91961, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8039
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8039
    Throughput:     3.83MB/s
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
    Reqs/sec     22403.86    6027.61   72940.78
    Latency        2.23ms     1.95ms   168.81ms
    HTTP codes:
      1xx - 0, 2xx - 96789, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3211
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3211
    Throughput:     4.97MB/s
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
    Reqs/sec     71730.38    3752.29   83257.05
    Latency      694.64us   158.92us     5.20ms
    HTTP codes:
      1xx - 0, 2xx - 97419, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2581
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2581
    Throughput:    11.06MB/s
  ```


