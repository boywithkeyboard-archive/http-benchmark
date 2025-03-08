## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73727` | `5335` | `89058` |
| **83%** | [Hyper Express](#hyper-express) | `61441` | `3316` | `64211` |
| **33%** | [Node (Default)](#node-default) | `24539` | `6865` | `55846` |
| **29%** | [Fastify](#fastify) | `21661` | `4940` | `36014` |
| **24%** | [Hono](#hono) | `17933` | `4040` | `28703` |
| **23%** | [Koa](#koa) | `16617` | `5256` | `48812` |
| **11%** | [Carbon](#carbon) | `8228` | `1396` | `10232` |
| **9%** | [Express](#express) | `6342` | `990` | `8339` |


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
    Reqs/sec      8635.76    4330.18   53743.65
    Latency        5.77ms     4.29ms   373.60ms
    HTTP codes:
      1xx - 0, 2xx - 94022, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5978
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5978
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
    Reqs/sec      6335.52     987.92    8307.06
    Latency        7.89ms     3.60ms   346.11ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     21254.47    5181.49   36272.80
    Latency        2.35ms     2.11ms   190.44ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.83MB/s
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
    Reqs/sec     17956.89    3698.42   28761.77
    Latency        2.78ms     2.05ms   182.70ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.06MB/s
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
    Reqs/sec     62061.86    3626.40   65823.61
    Latency      803.49us    72.89us     2.98ms
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
    Reqs/sec     17354.92    6338.77   58103.99
    Latency        2.87ms     2.45ms   211.41ms
    HTTP codes:
      1xx - 0, 2xx - 93354, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6646
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6646
    Throughput:     3.67MB/s
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
    Reqs/sec     24424.28    6811.32   53822.37
    Latency        2.04ms     1.88ms   164.85ms
    HTTP codes:
      1xx - 0, 2xx - 97611, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2389
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2389
    Throughput:     5.46MB/s
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
    Reqs/sec     73589.93    5460.75   87399.84
    Latency      676.85us   159.20us     7.52ms
    HTTP codes:
      1xx - 0, 2xx - 97732, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2268
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2268
    Throughput:    11.38MB/s
  ```


