## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75246` | `2243` | `79192` |
| **86%** | [Hyper Express](#hyper-express) | `64564` | `4613` | `69144` |
| **34%** | [Node (Default)](#node-default) | `25402` | `7955` | `66130` |
| **32%** | [Fastify](#fastify) | `23932` | `7840` | `36070` |
| **27%** | [Hono](#hono) | `20331` | `4993` | `29202` |
| **26%** | [Koa](#koa) | `19474` | `9425` | `79983` |
| **11%** | [Carbon](#carbon) | `8302` | `1487` | `10464` |
| **9%** | [Express](#express) | `6431` | `1083` | `8511` |


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
    Reqs/sec      8984.42    5658.51   71179.88
    Latency        5.55ms     4.39ms   383.96ms
    HTTP codes:
      1xx - 0, 2xx - 91992, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8008
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8008
    Throughput:     1.88MB/s
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
    Reqs/sec      6939.43    5203.26   63342.42
    Latency        7.19ms     3.80ms   348.44ms
    HTTP codes:
      1xx - 0, 2xx - 91224, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8776
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8776
    Throughput:     1.81MB/s
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
    Reqs/sec     24984.30    7733.05   36152.36
    Latency        2.00ms     2.04ms   184.28ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.67MB/s
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
    Reqs/sec     21279.15    5783.67   30189.23
    Latency        2.35ms     2.06ms   181.74ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.81MB/s
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
    Reqs/sec     62991.26    2988.61   65479.53
    Latency      791.77us    71.72us     3.55ms
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
    Reqs/sec     18950.44    7889.17   68737.77
    Latency        2.64ms     2.39ms   210.59ms
    HTTP codes:
      1xx - 0, 2xx - 93078, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6922
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6922
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
    Reqs/sec     24946.54    7243.06   59167.83
    Latency        2.00ms     1.90ms   165.79ms
    HTTP codes:
      1xx - 0, 2xx - 97626, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2374
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2374
    Throughput:     5.58MB/s
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
    Reqs/sec     75008.40    2384.10   81325.93
    Latency      664.33us   140.38us     7.97ms
    HTTP codes:
      1xx - 0, 2xx - 97169, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2831
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2831
    Throughput:    11.54MB/s
  ```


