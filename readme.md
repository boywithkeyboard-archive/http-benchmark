## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `82292` | `2438` | `86806` |
| **85%** | [Hyper Express](#hyper-express) | `70082` | `3429` | `72825` |
| **45%** | [Node (Default)](#node-default) | `37424` | `10637` | `79524` |
| **43%** | [Fastify](#fastify) | `35578` | `10931` | `51352` |
| **37%** | [Hono](#hono) | `30525` | `8903` | `47826` |
| **37%** | [Koa](#koa) | `30466` | `12386` | `76566` |
| **11%** | [Carbon](#carbon) | `9375` | `2371` | `13640` |
| **9%** | [Express](#express) | `7696` | `1757` | `10775` |


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
    Reqs/sec     10516.71    8959.97   86586.97
    Latency        4.75ms     4.26ms   369.76ms
    HTTP codes:
      1xx - 0, 2xx - 87762, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12238
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12238
    Throughput:     2.10MB/s
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
    Reqs/sec      8144.81    7207.54   77472.60
    Latency        6.13ms     3.84ms   352.59ms
    HTTP codes:
      1xx - 0, 2xx - 88985, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11015
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11015
    Throughput:     2.08MB/s
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
    Reqs/sec     35468.27   12318.20   52427.53
    Latency        1.41ms     2.08ms   184.07ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.05MB/s
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
    Reqs/sec     30159.93    9516.74   48677.56
    Latency        1.66ms     2.12ms   186.90ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     6.82MB/s
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
    Reqs/sec     70381.07    3449.71   78872.62
    Latency      709.79us    62.94us     2.75ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.98MB/s
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
    Reqs/sec     29974.41   12282.19   77164.24
    Latency        1.66ms     2.23ms   192.20ms
    HTTP codes:
      1xx - 0, 2xx - 92448, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7552
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7552
    Throughput:     6.27MB/s
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
    Reqs/sec     37314.52   11644.65   83176.08
    Latency        1.34ms     1.77ms   151.47ms
    HTTP codes:
      1xx - 0, 2xx - 95407, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4593
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4593
    Throughput:     8.16MB/s
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
    Reqs/sec     82511.31    2666.41   88437.12
    Latency      603.25us   157.92us     7.57ms
    HTTP codes:
      1xx - 0, 2xx - 96524, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3476
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3476
    Throughput:    12.61MB/s
  ```


