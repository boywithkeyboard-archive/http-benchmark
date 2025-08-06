## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `77546` | `4250` | `87649` |
| **80%** | [Hyper Express](#hyper-express) | `62201` | `3501` | `65572` |
| **33%** | [Node (Default)](#node-default) | `25560` | `7799` | `54981` |
| **30%** | [Fastify](#fastify) | `23604` | `7180` | `36875` |
| **26%** | [Hono](#hono) | `20513` | `5858` | `29821` |
| **25%** | [Koa](#koa) | `19189` | `9548` | `78679` |
| **11%** | [Carbon](#carbon) | `8224` | `1459` | `10375` |
| **8%** | [Express](#express) | `6314` | `1055` | `8317` |


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
    Reqs/sec      8768.42    5457.36   67693.23
    Latency        5.69ms     4.32ms   372.38ms
    HTTP codes:
      1xx - 0, 2xx - 92864, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7136
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7136
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
    Reqs/sec      6371.18    1058.42    8409.03
    Latency        7.84ms     3.60ms   347.46ms
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
    Reqs/sec     23751.64    7390.05   35984.06
    Latency        2.10ms     1.97ms   180.73ms
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
    Reqs/sec     21049.02    6094.45   30207.01
    Latency        2.37ms     2.04ms   183.96ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.75MB/s
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
    Reqs/sec     62217.84    3173.19   65561.52
    Latency      801.41us    79.66us     4.73ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.84MB/s
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
    Reqs/sec     18876.94    8755.05   73942.12
    Latency        2.64ms     2.43ms   213.52ms
    HTTP codes:
      1xx - 0, 2xx - 91685, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8315
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8315
    Throughput:     3.91MB/s
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
    Reqs/sec     25909.51    8432.65   63342.05
    Latency        1.92ms     1.94ms   165.81ms
    HTTP codes:
      1xx - 0, 2xx - 97353, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2647
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2647
    Throughput:     5.78MB/s
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
    Reqs/sec     76838.21    3288.19   82597.77
    Latency      648.06us   174.73us     8.96ms
    HTTP codes:
      1xx - 0, 2xx - 96544, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3456
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3456
    Throughput:    11.73MB/s
  ```


