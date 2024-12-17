## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75777` | `6858` | `87368` |
| **83%** | [Hyper Express](#hyper-express) | `62697` | `4019` | `84090` |
| **34%** | [Node (Default)](#node-default) | `25806` | `7675` | `56837` |
| **32%** | [Fastify](#fastify) | `24258` | `7210` | `36096` |
| **28%** | [Hono](#hono) | `21328` | `5663` | `30305` |
| **24%** | [Koa](#koa) | `18304` | `7341` | `55389` |
| **11%** | [Carbon](#carbon) | `8322` | `1445` | `10581` |
| **9%** | [Express](#express) | `6577` | `1059` | `8485` |


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
    Reqs/sec      8803.80    4236.28   54343.75
    Latency        5.67ms     4.15ms   360.23ms
    HTTP codes:
      1xx - 0, 2xx - 93996, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6004
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6004
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
    Reqs/sec      6721.48    1113.09    8628.81
    Latency        7.44ms     3.46ms   333.09ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.92MB/s
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
    Reqs/sec     24354.86    7229.19   35803.47
    Latency        2.05ms     1.80ms   165.72ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.52MB/s
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
    Reqs/sec     21466.21    5992.63   30442.04
    Latency        2.33ms     2.00ms   180.78ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.84MB/s
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
    Reqs/sec     62652.32    3094.43   69437.93
    Latency      795.17us    68.57us     2.84ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.90MB/s
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
    Reqs/sec     18662.05    6741.66   61238.86
    Latency        2.67ms     2.30ms   200.61ms
    HTTP codes:
      1xx - 0, 2xx - 93306, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6694
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6694
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
    Reqs/sec     26086.33    7543.36   41513.67
    Latency        1.92ms     1.78ms   154.67ms
    HTTP codes:
      1xx - 0, 2xx - 98306, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1694
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1694
    Throughput:     5.86MB/s
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
    Reqs/sec     74342.58    5749.58   84077.43
    Latency      670.76us   227.02us    16.71ms
    HTTP codes:
      1xx - 0, 2xx - 97880, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2120
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2120
    Throughput:    11.50MB/s
  ```


