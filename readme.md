## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73068` | `4883` | `83899` |
| **84%** | [Hyper Express](#hyper-express) | `61331` | `3264` | `68712` |
| **37%** | [Node (Default)](#node-default) | `26904` | `7658` | `50874` |
| **32%** | [Fastify](#fastify) | `23331` | `6820` | `36802` |
| **30%** | [Hono](#hono) | `22087` | `6352` | `30524` |
| **26%** | [Koa](#koa) | `19259` | `6524` | `49815` |
| **11%** | [Carbon](#carbon) | `8244` | `1354` | `10212` |
| **9%** | [Express](#express) | `6363` | `975` | `8375` |


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
    Reqs/sec      8747.18    4337.19   52029.74
    Latency        5.71ms     4.32ms   369.90ms
    HTTP codes:
      1xx - 0, 2xx - 94059, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5941
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5941
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
    Reqs/sec      6879.96    4254.17   52626.73
    Latency        7.26ms     3.92ms   358.11ms
    HTTP codes:
      1xx - 0, 2xx - 92577, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7423
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7423
    Throughput:     1.82MB/s
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
    Reqs/sec     24393.68    6842.22   36158.93
    Latency        2.05ms     2.01ms   179.56ms
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
    Reqs/sec     20926.01    5931.89   29892.76
    Latency        2.39ms     2.03ms   184.03ms
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
    Reqs/sec     63692.65    4077.60   71394.45
    Latency      782.78us    70.71us     3.42ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.04MB/s
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
    Reqs/sec     18824.85    6491.84   51141.42
    Latency        2.65ms     2.35ms   206.85ms
    HTTP codes:
      1xx - 0, 2xx - 94003, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5997
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5997
    Throughput:     4.00MB/s
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
    Reqs/sec     26706.36    7356.41   50415.73
    Latency        1.87ms     1.85ms   158.71ms
    HTTP codes:
      1xx - 0, 2xx - 98048, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1952
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1952
    Throughput:     5.99MB/s
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
    Reqs/sec     72965.86    4706.60   78990.02
    Latency      682.59us   184.12us     8.67ms
    HTTP codes:
      1xx - 0, 2xx - 96948, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3052
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3052
    Throughput:    11.19MB/s
  ```


