## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73817` | `2707` | `80646` |
| **83%** | [Hyper Express](#hyper-express) | `61243` | `3781` | `63779` |
| **34%** | [Node (Default)](#node-default) | `25284` | `8203` | `62472` |
| **33%** | [Fastify](#fastify) | `24639` | `7954` | `36214` |
| **28%** | [Hono](#hono) | `20676` | `6107` | `29504` |
| **25%** | [Koa](#koa) | `18644` | `8399` | `71919` |
| **11%** | [Carbon](#carbon) | `8228` | `1473` | `10416` |
| **9%** | [Express](#express) | `6422` | `1054` | `8355` |


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
    Reqs/sec      8775.61    5530.15   67325.46
    Latency        5.69ms     4.31ms   371.33ms
    HTTP codes:
      1xx - 0, 2xx - 92430, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7570
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7570
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
    Reqs/sec      6448.66    1098.56    8403.42
    Latency        7.75ms     3.75ms   355.81ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.84MB/s
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
    Reqs/sec     23851.28    7255.68   35260.19
    Latency        2.09ms     2.07ms   184.80ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.41MB/s
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
    Reqs/sec     19927.73    5436.61   28991.16
    Latency        2.51ms     2.09ms   186.85ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.50MB/s
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
    Reqs/sec     62128.70    3817.11   67224.63
    Latency      802.49us    70.14us     3.04ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.83MB/s
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
    Reqs/sec     18302.53    7217.65   63938.94
    Latency        2.73ms     2.40ms   211.85ms
    HTTP codes:
      1xx - 0, 2xx - 93400, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6600
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6600
    Throughput:     3.86MB/s
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
    Reqs/sec     25186.32    8547.08   76365.73
    Latency        1.98ms     1.87ms   156.33ms
    HTTP codes:
      1xx - 0, 2xx - 96674, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3326
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3326
    Throughput:     5.57MB/s
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
    Reqs/sec     74842.83    2502.58   87838.06
    Latency      664.90us   178.39us     8.83ms
    HTTP codes:
      1xx - 0, 2xx - 95805, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4195
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4195
    Throughput:    11.35MB/s
  ```


