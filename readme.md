## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73230` | `5083` | `82621` |
| **83%** | [Hyper Express](#hyper-express) | `60964` | `3440` | `64121` |
| **35%** | [Node (Default)](#node-default) | `25923` | `7694` | `48393` |
| **31%** | [Fastify](#fastify) | `22956` | `7151` | `35417` |
| **28%** | [Hono](#hono) | `20231` | `5873` | `29826` |
| **25%** | [Koa](#koa) | `18336` | `7409` | `64642` |
| **11%** | [Carbon](#carbon) | `8019` | `1410` | `10184` |
| **9%** | [Express](#express) | `6296` | `1000` | `8288` |


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
    Reqs/sec      8468.68    3751.74   49276.14
    Latency        5.90ms     4.42ms   380.33ms
    HTTP codes:
      1xx - 0, 2xx - 94733, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5267
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5267
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
    Reqs/sec      6212.15    1005.63    8226.17
    Latency        8.05ms     3.82ms   367.80ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     23731.86    7589.47   37947.27
    Latency        2.10ms     2.07ms   185.12ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.39MB/s
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
    Reqs/sec     20666.32    6367.70   30098.62
    Latency        2.42ms     2.12ms   188.28ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.67MB/s
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
    Reqs/sec     61251.12    3460.28   64204.86
    Latency      813.78us    77.65us     4.87ms
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
    Reqs/sec     18666.24    6476.63   51015.58
    Latency        2.67ms     2.40ms   210.83ms
    HTTP codes:
      1xx - 0, 2xx - 94380, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5620
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5620
    Throughput:     3.98MB/s
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
    Reqs/sec     24526.61    6403.07   45942.00
    Latency        2.04ms     1.81ms   161.31ms
    HTTP codes:
      1xx - 0, 2xx - 98119, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1881
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1881
    Throughput:     5.51MB/s
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
    Reqs/sec     72285.79    4569.90   81520.64
    Latency      688.63us   167.01us     5.66ms
    HTTP codes:
      1xx - 0, 2xx - 96895, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3105
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3105
    Throughput:    11.09MB/s
  ```


