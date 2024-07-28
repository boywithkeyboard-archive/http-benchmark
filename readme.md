## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73629` | `6130` | `90154` |
| **85%** | [Hyper Express](#hyper-express) | `62688` | `4036` | `73907` |
| **36%** | [Node (Default)](#node-default) | `26697` | `8117` | `42214` |
| **32%** | [Fastify](#fastify) | `23609` | `7109` | `35710` |
| **29%** | [Hono](#hono) | `21069` | `6117` | `29866` |
| **24%** | [Koa](#koa) | `17595` | `6474` | `55955` |
| **11%** | [Carbon](#carbon) | `8015` | `1339` | `10319` |
| **9%** | [Express](#express) | `6476` | `1018` | `8489` |


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
    Reqs/sec      8355.56    4051.82   55011.97
    Latency        5.98ms     4.19ms   360.60ms
    HTTP codes:
      1xx - 0, 2xx - 94000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6000
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6000
    Throughput:     1.78MB/s
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
    Reqs/sec      6411.01    1036.44    8385.81
    Latency        7.80ms     3.73ms   356.48ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.83MB/s
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
    Reqs/sec     24412.23    7634.02   35668.96
    Latency        2.05ms     2.03ms   183.47ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.54MB/s
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
    Reqs/sec     20268.65    5340.05   29527.82
    Latency        2.47ms     1.97ms   178.56ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.58MB/s
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
    Reqs/sec     62564.80    3668.90   65436.38
    Latency      797.02us    71.96us     3.51ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.89MB/s
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
    Reqs/sec     17666.16    5955.37   57159.00
    Latency        2.82ms     2.41ms   209.50ms
    HTTP codes:
      1xx - 0, 2xx - 94991, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5009
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5009
    Throughput:     3.80MB/s
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
    Reqs/sec     25796.41    7802.71   57700.20
    Latency        1.93ms     1.81ms   161.56ms
    HTTP codes:
      1xx - 0, 2xx - 96959, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3041
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3041
    Throughput:     5.73MB/s
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
    Reqs/sec     73252.88    7896.37   91601.13
    Latency      682.15us   331.86us    20.48ms
    HTTP codes:
      1xx - 0, 2xx - 98248, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1752
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1752
    Throughput:    11.36MB/s
  ```


