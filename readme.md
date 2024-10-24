## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74556` | `6060` | `81668` |
| **83%** | [Hyper Express](#hyper-express) | `62224` | `3890` | `68267` |
| **36%** | [Node (Default)](#node-default) | `26627` | `7813` | `54296` |
| **33%** | [Fastify](#fastify) | `24506` | `7290` | `35370` |
| **28%** | [Hono](#hono) | `20634` | `5728` | `29023` |
| **24%** | [Koa](#koa) | `18150` | `7013` | `60861` |
| **11%** | [Carbon](#carbon) | `8024` | `1363` | `10263` |
| **9%** | [Express](#express) | `6446` | `1057` | `8583` |


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
    Reqs/sec      8655.25    3932.68   59047.55
    Latency        5.77ms     4.33ms   371.83ms
    HTTP codes:
      1xx - 0, 2xx - 94788, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5212
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5212
    Throughput:     1.86MB/s
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
    Reqs/sec      6442.53    1052.12    8468.33
    Latency        7.76ms     3.62ms   346.29ms
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
    Reqs/sec     23320.99    6781.47   34952.30
    Latency        2.14ms     1.98ms   178.01ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.29MB/s
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
    Reqs/sec     20681.64    5629.20   29008.12
    Latency        2.41ms     2.05ms   184.52ms
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
    Reqs/sec     62952.31    4117.86   75988.25
    Latency      791.48us    83.28us     3.60ms
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
    Reqs/sec     18391.56    6336.52   53011.24
    Latency        2.71ms     2.40ms   206.45ms
    HTTP codes:
      1xx - 0, 2xx - 94695, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5305
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5305
    Throughput:     3.94MB/s
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
    Reqs/sec     26228.52    7819.07   48944.15
    Latency        1.90ms     1.92ms   168.12ms
    HTTP codes:
      1xx - 0, 2xx - 97123, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2877
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2877
    Throughput:     5.83MB/s
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
    Reqs/sec     74615.32    5981.64   83389.69
    Latency      667.75us   193.39us    15.84ms
    HTTP codes:
      1xx - 0, 2xx - 98080, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1920
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1920
    Throughput:    11.58MB/s
  ```


