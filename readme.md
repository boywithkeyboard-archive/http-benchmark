## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74300` | `5635` | `80856` |
| **85%** | [Hyper Express](#hyper-express) | `63351` | `3920` | `77197` |
| **33%** | [Node (Default)](#node-default) | `24411` | `7354` | `68761` |
| **32%** | [Fastify](#fastify) | `24065` | `7223` | `35720` |
| **27%** | [Hono](#hono) | `20285` | `5351` | `28573` |
| **24%** | [Koa](#koa) | `17826` | `5944` | `45473` |
| **11%** | [Carbon](#carbon) | `8149` | `1380` | `10403` |
| **9%** | [Express](#express) | `6564` | `1060` | `8527` |


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
    Reqs/sec      8584.57    3742.49   51797.90
    Latency        5.77ms     4.25ms   370.68ms
    HTTP codes:
      1xx - 0, 2xx - 94601, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5399
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5399
    Throughput:     1.86MB/s
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
    Reqs/sec      6551.43    1038.26    8479.13
    Latency        7.63ms     3.53ms   337.14ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.87MB/s
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
    Reqs/sec     24971.02    7309.36   35958.54
    Latency        2.00ms     2.00ms   180.14ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.66MB/s
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
    Reqs/sec     20877.43    5743.63   30020.70
    Latency        2.39ms     2.09ms   186.75ms
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
    Reqs/sec     63332.73    4160.27   75731.74
    Latency      787.42us    72.89us     2.87ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.00MB/s
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
    Reqs/sec     16909.81    6078.82   52852.25
    Latency        2.95ms     2.33ms   203.47ms
    HTTP codes:
      1xx - 0, 2xx - 93899, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6101
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6101
    Throughput:     3.59MB/s
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
    Reqs/sec     24982.19    6881.20   44268.64
    Latency        2.00ms     1.90ms   163.24ms
    HTTP codes:
      1xx - 0, 2xx - 98206, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1794
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1794
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
    Reqs/sec     74907.01    6286.09   88346.27
    Latency      665.46us   168.69us    11.86ms
    HTTP codes:
      1xx - 0, 2xx - 98174, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1826
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1826
    Throughput:    11.63MB/s
  ```


