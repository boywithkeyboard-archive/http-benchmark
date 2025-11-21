## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74739` | `3057` | `82310` |
| **83%** | [Hyper Express](#hyper-express) | `62086` | `3027` | `65406` |
| **33%** | [Node (Default)](#node-default) | `24597` | `7602` | `58586` |
| **33%** | [Fastify](#fastify) | `24477` | `8058` | `36036` |
| **27%** | [Hono](#hono) | `20341` | `5718` | `29729` |
| **25%** | [Koa](#koa) | `18376` | `9250` | `75202` |
| **11%** | [Carbon](#carbon) | `8071` | `1414` | `10232` |
| **8%** | [Express](#express) | `6326` | `1090` | `8345` |


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
    Reqs/sec      8923.87    6600.01   79513.68
    Latency        5.59ms     4.40ms   376.00ms
    HTTP codes:
      1xx - 0, 2xx - 90780, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9220
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9220
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
    Reqs/sec      6235.52     999.04    8324.94
    Latency        8.01ms     3.74ms   356.81ms
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
    Reqs/sec     23054.95    7228.09   34917.53
    Latency        2.17ms     2.03ms   182.66ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.23MB/s
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
    Reqs/sec     19987.34    5501.87   29499.64
    Latency        2.50ms     2.00ms   183.18ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.52MB/s
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
    Reqs/sec     62072.06    3336.33   64745.24
    Latency      803.07us    68.15us     2.94ms
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
    Reqs/sec     18621.17    8824.72   79771.14
    Latency        2.68ms     2.39ms   208.75ms
    HTTP codes:
      1xx - 0, 2xx - 91497, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8503
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8503
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
    Reqs/sec     25811.59    8361.06   61969.66
    Latency        1.93ms     1.81ms   155.70ms
    HTTP codes:
      1xx - 0, 2xx - 97480, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2520
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2520
    Throughput:     5.77MB/s
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
    Reqs/sec     74586.22    3026.10   83345.57
    Latency      668.10us   115.68us     5.21ms
    HTTP codes:
      1xx - 0, 2xx - 97535, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2465
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2465
    Throughput:    11.50MB/s
  ```


