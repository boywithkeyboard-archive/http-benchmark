## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73559` | `5640` | `78232` |
| **83%** | [Hyper Express](#hyper-express) | `60837` | `3982` | `66486` |
| **34%** | [Node (Default)](#node-default) | `25136` | `7038` | `41511` |
| **33%** | [Fastify](#fastify) | `24331` | `7262` | `35633` |
| **28%** | [Hono](#hono) | `20278` | `5654` | `30408` |
| **24%** | [Koa](#koa) | `17643` | `6416` | `53926` |
| **11%** | [Carbon](#carbon) | `7982` | `1313` | `10442` |
| **9%** | [Express](#express) | `6392` | `1012` | `8533` |


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
    Reqs/sec      8352.00    3107.10   43768.54
    Latency        5.95ms     4.26ms   369.73ms
    HTTP codes:
      1xx - 0, 2xx - 95780, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4220
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4220
    Throughput:     1.82MB/s
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
    Reqs/sec      6171.89     913.33    8406.66
    Latency        8.10ms     3.66ms   350.12ms
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
    Reqs/sec     23717.78    7049.97   36741.06
    Latency        2.11ms     2.09ms   188.62ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.38MB/s
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
    Reqs/sec     21276.64    6212.53   30518.23
    Latency        2.35ms     2.02ms   180.19ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.80MB/s
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
    Reqs/sec     62275.36    3579.24   67265.58
    Latency      799.59us    73.30us     4.47ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.86MB/s
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
    Reqs/sec     18373.05    6351.65   49311.97
    Latency        2.72ms     2.37ms   208.18ms
    HTTP codes:
      1xx - 0, 2xx - 94482, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5518
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5518
    Throughput:     3.92MB/s
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
    Reqs/sec     25050.31    6962.62   47920.97
    Latency        1.99ms     1.95ms   169.51ms
    HTTP codes:
      1xx - 0, 2xx - 97509, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2491
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2491
    Throughput:     5.61MB/s
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
    Reqs/sec     73264.09    7286.10   92089.35
    Latency      677.57us   144.37us    11.01ms
    HTTP codes:
      1xx - 0, 2xx - 98218, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1782
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1782
    Throughput:    11.39MB/s
  ```


