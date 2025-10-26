## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `76626` | `3860` | `96392` |
| **82%** | [Hyper Express](#hyper-express) | `63086` | `4012` | `71791` |
| **33%** | [Node (Default)](#node-default) | `25484` | `8201` | `67452` |
| **31%** | [Fastify](#fastify) | `23796` | `7519` | `36413` |
| **26%** | [Hono](#hono) | `20271` | `6005` | `29880` |
| **23%** | [Koa](#koa) | `17994` | `7795` | `67221` |
| **11%** | [Carbon](#carbon) | `8068` | `1457` | `10308` |
| **8%** | [Express](#express) | `6189` | `955` | `8215` |


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
    Reqs/sec      8782.77    5879.52   73099.06
    Latency        5.68ms     4.36ms   376.65ms
    HTTP codes:
      1xx - 0, 2xx - 91771, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8229
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8229
    Throughput:     1.83MB/s
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
    Reqs/sec      6276.99    1024.21    8245.56
    Latency        7.96ms     3.75ms   360.36ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.80MB/s
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
    Reqs/sec     23011.96    6960.46   35280.57
    Latency        2.17ms     2.06ms   184.94ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.22MB/s
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
    Reqs/sec     20388.63    5937.05   29579.13
    Latency        2.45ms     2.07ms   183.74ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.60MB/s
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
    Reqs/sec     61381.80    3779.48   63904.33
    Latency      812.53us    78.59us     3.37ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.72MB/s
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
    Reqs/sec     18939.13    8396.58   76909.25
    Latency        2.63ms     2.35ms   209.69ms
    HTTP codes:
      1xx - 0, 2xx - 92560, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7440
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7440
    Throughput:     3.96MB/s
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
    Reqs/sec     24870.68    8130.34   77718.96
    Latency        2.00ms     1.91ms   162.03ms
    HTTP codes:
      1xx - 0, 2xx - 96684, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3316
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3316
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
    Reqs/sec     76034.08    4400.52  100827.90
    Latency      657.94us   136.17us     9.28ms
    HTTP codes:
      1xx - 0, 2xx - 97187, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2813
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2813
    Throughput:    11.64MB/s
  ```


