## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `78921` | `2392` | `84339` |
| **86%** | [Hyper Express](#hyper-express) | `68133` | `3681` | `71813` |
| **47%** | [Node (Default)](#node-default) | `36823` | `13160` | `80080` |
| **43%** | [Fastify](#fastify) | `34260` | `12397` | `50687` |
| **42%** | [Hono](#hono) | `33293` | `11463` | `45497` |
| **36%** | [Koa](#koa) | `28795` | `11095` | `69371` |
| **12%** | [Carbon](#carbon) | `9206` | `2246` | `13311` |
| **9%** | [Express](#express) | `7478` | `1706` | `10926` |


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
    Reqs/sec     10049.01    6233.15   73474.08
    Latency        4.96ms     4.35ms   369.51ms
    HTTP codes:
      1xx - 0, 2xx - 93066, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6934
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6934
    Throughput:     2.13MB/s
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
    Reqs/sec      8378.32    7177.75   88016.11
    Latency        5.96ms     3.77ms   340.41ms
    HTTP codes:
      1xx - 0, 2xx - 89306, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10694
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10694
    Throughput:     2.14MB/s
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
    Reqs/sec     37340.98   13498.89   51372.42
    Latency        1.34ms     2.01ms   173.97ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.45MB/s
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
    Reqs/sec     31582.51   10398.21   45829.04
    Latency        1.58ms     1.96ms   173.26ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.15MB/s
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
    Reqs/sec     68492.65    2869.29   72139.80
    Latency      728.11us    63.80us     2.77ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.73MB/s
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
    Reqs/sec     28897.65   11988.71   75290.84
    Latency        1.73ms     2.27ms   195.81ms
    HTTP codes:
      1xx - 0, 2xx - 92567, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7433
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7433
    Throughput:     6.04MB/s
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
    Reqs/sec     40198.07   13391.64   71523.79
    Latency        1.24ms     1.79ms   153.29ms
    HTTP codes:
      1xx - 0, 2xx - 96263, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3737
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3737
    Throughput:     8.86MB/s
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
    Reqs/sec     79770.92    1972.28   85008.37
    Latency      624.72us   152.23us     6.10ms
    HTTP codes:
      1xx - 0, 2xx - 95473, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4527
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4527
    Throughput:    12.05MB/s
  ```


