## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `71441` | `6119` | `92318` |
| **85%** | [Hyper Express](#hyper-express) | `60782` | `3500` | `63580` |
| **32%** | [Node (Default)](#node-default) | `22771` | `6139` | `63465` |
| **31%** | [Fastify](#fastify) | `22434` | `6728` | `36190` |
| **31%** | [Hono](#hono) | `22236` | `6572` | `30952` |
| **26%** | [Koa](#koa) | `18607` | `9998` | `81486` |
| **11%** | [Carbon](#carbon) | `7543` | `1152` | `10350` |
| **9%** | [Express](#express) | `6411` | `1084` | `8345` |


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
    Reqs/sec      8132.03    5837.51   76355.49
    Latency        6.14ms     4.35ms   369.75ms
    HTTP codes:
      1xx - 0, 2xx - 91468, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8532
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8532
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
    Reqs/sec      6356.24    1057.94    8316.87
    Latency        7.86ms     3.67ms   353.45ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     24089.31    7275.27   35473.19
    Latency        2.07ms     2.07ms   183.97ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.47MB/s
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
    Reqs/sec     22481.28    6762.13   30573.42
    Latency        2.22ms     2.14ms   191.24ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.08MB/s
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
    Reqs/sec     61263.72    3553.02   67461.52
    Latency      813.79us    77.60us     2.85ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.70MB/s
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
    Reqs/sec     19112.88    8297.78   65914.01
    Latency        2.61ms     2.42ms   211.55ms
    HTTP codes:
      1xx - 0, 2xx - 93172, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6828
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6828
    Throughput:     4.03MB/s
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
    Reqs/sec     24439.11    9256.45   81207.98
    Latency        2.04ms     1.97ms   169.76ms
    HTTP codes:
      1xx - 0, 2xx - 94148, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5852
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5852
    Throughput:     5.28MB/s
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
    Reqs/sec     73877.43    4305.00   84113.14
    Latency      673.66us   164.41us     5.75ms
    HTTP codes:
      1xx - 0, 2xx - 96793, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3207
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3207
    Throughput:    11.32MB/s
  ```


