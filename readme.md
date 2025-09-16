## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74370` | `2550` | `82845` |
| **83%** | [Hyper Express](#hyper-express) | `61474` | `3505` | `64449` |
| **36%** | [Node (Default)](#node-default) | `26802` | `8624` | `66759` |
| **30%** | [Fastify](#fastify) | `22627` | `6380` | `35682` |
| **28%** | [Hono](#hono) | `20770` | `5840` | `30452` |
| **26%** | [Koa](#koa) | `19000` | `8973` | `78069` |
| **11%** | [Carbon](#carbon) | `8189` | `1404` | `10383` |
| **8%** | [Express](#express) | `6285` | `1063` | `8330` |


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
    Reqs/sec      9162.52    7745.14   76988.12
    Latency        5.45ms     4.33ms   371.79ms
    HTTP codes:
      1xx - 0, 2xx - 88104, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11896
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11896
    Throughput:     1.83MB/s
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
    Reqs/sec      6984.50    6387.58   76275.98
    Latency        7.15ms     4.01ms   369.50ms
    HTTP codes:
      1xx - 0, 2xx - 88803, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11197
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11197
    Throughput:     1.78MB/s
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
    Reqs/sec     24200.40    7318.45   35897.06
    Latency        2.06ms     2.08ms   186.19ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.49MB/s
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
    Reqs/sec     21440.90    6435.45   30112.81
    Latency        2.33ms     2.05ms   183.97ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.85MB/s
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
    Reqs/sec     63307.03    2795.76   66737.98
    Latency      787.71us    72.11us     2.96ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.99MB/s
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
    Reqs/sec     19001.62    9408.41   84405.23
    Latency        2.62ms     2.38ms   211.52ms
    HTTP codes:
      1xx - 0, 2xx - 90637, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9363
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9363
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
    Reqs/sec     26177.96    8629.45   76429.72
    Latency        1.90ms     1.89ms   161.77ms
    HTTP codes:
      1xx - 0, 2xx - 96062, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3938
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3938
    Throughput:     5.76MB/s
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
    Reqs/sec     75555.28    2459.40   82132.15
    Latency      658.14us   172.43us    11.80ms
    HTTP codes:
      1xx - 0, 2xx - 95778, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4222
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4222
    Throughput:    11.46MB/s
  ```


