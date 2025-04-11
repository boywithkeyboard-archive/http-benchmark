## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74251` | `5083` | `82998` |
| **83%** | [Hyper Express](#hyper-express) | `61745` | `4218` | `76954` |
| **35%** | [Node (Default)](#node-default) | `26062` | `7327` | `55301` |
| **33%** | [Fastify](#fastify) | `24285` | `7284` | `36268` |
| **29%** | [Hono](#hono) | `21519` | `6236` | `30341` |
| **25%** | [Koa](#koa) | `18854` | `5367` | `41008` |
| **11%** | [Carbon](#carbon) | `8198` | `1363` | `10337` |
| **9%** | [Express](#express) | `6520` | `1067` | `8463` |


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
    Reqs/sec      8711.28    4697.99   54754.69
    Latency        5.73ms     4.27ms   367.89ms
    HTTP codes:
      1xx - 0, 2xx - 93085, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6915
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6915
    Throughput:     1.84MB/s
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
    Reqs/sec      6479.39    1060.98    8493.70
    Latency        7.71ms     3.57ms   345.44ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.85MB/s
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
    Reqs/sec     24088.31    7258.22   36267.81
    Latency        2.07ms     1.96ms   175.26ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.46MB/s
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
    Reqs/sec     20484.03    5859.48   29685.78
    Latency        2.44ms     2.07ms   183.06ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.63MB/s
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
    Reqs/sec     62043.72    3946.10   69377.14
    Latency      803.43us    76.66us     5.58ms
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
    Reqs/sec     19191.62    6897.59   58120.01
    Latency        2.60ms     2.39ms   210.15ms
    HTTP codes:
      1xx - 0, 2xx - 93270, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6730
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6730
    Throughput:     4.05MB/s
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
    Reqs/sec     27062.33    8590.74   58167.72
    Latency        1.84ms     1.94ms   166.89ms
    HTTP codes:
      1xx - 0, 2xx - 96879, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3121
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3121
    Throughput:     6.01MB/s
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
    Reqs/sec     74116.53    5643.08   83247.91
    Latency      672.56us   159.88us     6.48ms
    HTTP codes:
      1xx - 0, 2xx - 97633, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2367
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2367
    Throughput:    11.45MB/s
  ```


