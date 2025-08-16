## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74319` | `2133` | `77701` |
| **85%** | [Hyper Express](#hyper-express) | `62911` | `3228` | `67075` |
| **34%** | [Node (Default)](#node-default) | `25585` | `7729` | `63724` |
| **33%** | [Fastify](#fastify) | `24391` | `7807` | `36095` |
| **28%** | [Hono](#hono) | `21154` | `6015` | `30175` |
| **27%** | [Koa](#koa) | `19906` | `9022` | `70721` |
| **11%** | [Carbon](#carbon) | `8512` | `1489` | `10414` |
| **9%** | [Express](#express) | `6507` | `1075` | `8375` |


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
    Reqs/sec      9019.80    7232.13   83868.98
    Latency        5.53ms     4.30ms   370.56ms
    HTTP codes:
      1xx - 0, 2xx - 89015, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10985
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10985
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
    Reqs/sec      7121.31    5482.37   66159.27
    Latency        7.01ms     3.91ms   360.69ms
    HTTP codes:
      1xx - 0, 2xx - 91266, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8734
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8734
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
    Reqs/sec     24445.59    7592.39   35898.37
    Latency        2.04ms     1.95ms   174.56ms
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
    Reqs/sec     20611.45    5426.88   29576.05
    Latency        2.42ms     1.98ms   181.23ms
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
    Reqs/sec     62949.15    3757.28   66843.46
    Latency      791.76us    64.17us     2.56ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.95MB/s
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
    Reqs/sec     18644.22    7874.32   67456.35
    Latency        2.67ms     2.36ms   206.56ms
    HTTP codes:
      1xx - 0, 2xx - 93119, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6881
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6881
    Throughput:     3.94MB/s
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
    Reqs/sec     25906.51    8087.09   67391.50
    Latency        1.93ms     2.01ms   170.30ms
    HTTP codes:
      1xx - 0, 2xx - 96611, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3389
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3389
    Throughput:     5.72MB/s
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
    Reqs/sec     74869.96    2616.67   82080.49
    Latency      664.72us   170.02us    10.35ms
    HTTP codes:
      1xx - 0, 2xx - 96285, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3715
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3715
    Throughput:    11.40MB/s
  ```


