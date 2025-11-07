## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `123154` | `5207` | `130938` |
| **80%** | [Hyper Express](#hyper-express) | `99010` | `6573` | `103772` |
| **31%** | [Node (Default)](#node-default) | `37698` | `10130` | `97551` |
| **28%** | [Fastify](#fastify) | `34412` | `7654` | `44177` |
| **26%** | [Koa](#koa) | `31864` | `14186` | `104562` |
| **25%** | [Hono](#hono) | `31198` | `6832` | `47194` |
| **10%** | [Carbon](#carbon) | `11958` | `2441` | `16263` |
| **7%** | [Express](#express) | `8484` | `1644` | `11435` |


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
    Reqs/sec     13439.61   10425.62  106798.65
    Latency        3.71ms     3.78ms   322.98ms
    HTTP codes:
      1xx - 0, 2xx - 88966, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11034
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11034
    Throughput:     2.72MB/s
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
    Reqs/sec      9618.75   10500.37  110644.97
    Latency        5.19ms     3.46ms   315.68ms
    HTTP codes:
      1xx - 0, 2xx - 85549, 3xx - 0, 4xx - 0, 5xx - 0
      others - 14451
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 14451
    Throughput:     2.36MB/s
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
    Reqs/sec     35005.93    8081.17   54276.73
    Latency        1.43ms     1.47ms   129.17ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.94MB/s
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
    Reqs/sec     31136.03    6878.15   46664.81
    Latency        1.60ms     1.52ms   134.76ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.04MB/s
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
    Reqs/sec     99204.75    7036.06  106082.70
    Latency      502.31us    52.69us     4.07ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    14.08MB/s
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
    Reqs/sec     30254.15   13894.90  104955.85
    Latency        1.65ms     1.86ms   154.12ms
    HTTP codes:
      1xx - 0, 2xx - 89663, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10337
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10337
    Throughput:     6.13MB/s
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
    Reqs/sec     37024.18   11360.89  113313.24
    Latency        1.34ms     1.38ms   115.93ms
    HTTP codes:
      1xx - 0, 2xx - 94651, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5349
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5349
    Throughput:     8.04MB/s
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
    Reqs/sec    126715.98    9702.16  142445.35
    Latency      391.62us   139.28us     6.40ms
    HTTP codes:
      1xx - 0, 2xx - 96738, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3262
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3262
    Throughput:    19.40MB/s
  ```


