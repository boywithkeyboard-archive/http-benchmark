## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75081` | `5127` | `83989` |
| **84%** | [Hyper Express](#hyper-express) | `63299` | `3884` | `67975` |
| **36%** | [Node (Default)](#node-default) | `27103` | `8206` | `47383` |
| **33%** | [Fastify](#fastify) | `25080` | `7572` | `37015` |
| **28%** | [Hono](#hono) | `21047` | `6121` | `31596` |
| **26%** | [Koa](#koa) | `19303` | `6635` | `52133` |
| **11%** | [Carbon](#carbon) | `8300` | `1426` | `10308` |
| **9%** | [Express](#express) | `6461` | `1023` | `8498` |


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
    Reqs/sec      8899.22    5453.81   63095.94
    Latency        5.61ms     4.20ms   366.15ms
    HTTP codes:
      1xx - 0, 2xx - 91501, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8499
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8499
    Throughput:     1.85MB/s
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
    Reqs/sec      7096.30    4869.12   66090.03
    Latency        7.04ms     3.93ms   358.62ms
    HTTP codes:
      1xx - 0, 2xx - 91799, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8201
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8201
    Throughput:     1.86MB/s
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
    Reqs/sec     25238.07    7852.05   38203.05
    Latency        1.98ms     1.98ms   176.72ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.72MB/s
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
    Reqs/sec     20982.81    5835.98   30102.05
    Latency        2.38ms     1.97ms   178.74ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.74MB/s
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
    Reqs/sec     62092.21    3638.30   64705.40
    Latency      802.84us    75.96us     3.03ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.82MB/s
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
    Reqs/sec     19117.23    7363.46   57247.47
    Latency        2.61ms     2.35ms   207.05ms
    HTTP codes:
      1xx - 0, 2xx - 93438, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6562
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6562
    Throughput:     4.04MB/s
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
    Reqs/sec     26032.69    7024.57   49306.15
    Latency        1.92ms     1.84ms   160.05ms
    HTTP codes:
      1xx - 0, 2xx - 98019, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1981
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1981
    Throughput:     5.84MB/s
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
    Reqs/sec     74673.68    5693.41   87444.69
    Latency      669.08us   148.97us     8.88ms
    HTTP codes:
      1xx - 0, 2xx - 98044, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1956
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1956
    Throughput:    11.55MB/s
  ```


