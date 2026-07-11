## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `76561` | `3471` | `82307` |
| **89%** | [Hyper Express](#hyper-express) | `67884` | `3421` | `70989` |
| **45%** | [Node (Default)](#node-default) | `34392` | `9575` | `74627` |
| **42%** | [Fastify](#fastify) | `32082` | `8277` | `51513` |
| **38%** | [Hono](#hono) | `28939` | `9060` | `46836` |
| **38%** | [Koa](#koa) | `28770` | `14427` | `96103` |
| **12%** | [Carbon](#carbon) | `9069` | `2194` | `13046` |
| **9%** | [Express](#express) | `7224` | `1651` | `10565` |


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
    Reqs/sec      9796.35    7611.85   86415.51
    Latency        5.09ms     4.64ms   410.30ms
    HTTP codes:
      1xx - 0, 2xx - 89775, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10225
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10225
    Throughput:     2.00MB/s
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
    Reqs/sec      8064.29    7137.57   79504.67
    Latency        6.18ms     3.82ms   345.98ms
    HTTP codes:
      1xx - 0, 2xx - 88942, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11058
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11049
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 9
    Throughput:     2.06MB/s
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
    Reqs/sec     32936.98    9894.17   51758.46
    Latency        1.52ms     1.93ms   171.02ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.47MB/s
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
    Reqs/sec     29533.60    9392.23   46194.99
    Latency        1.69ms     2.04ms   178.95ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     6.66MB/s
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
    Reqs/sec     68335.17    3550.57   71576.14
    Latency      729.49us    81.54us     2.92ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.71MB/s
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
    Reqs/sec     30286.97   12797.90   73526.13
    Latency        1.65ms     2.23ms   193.71ms
    HTTP codes:
      1xx - 0, 2xx - 92549, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7451
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7451
    Throughput:     6.33MB/s
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
    Reqs/sec     34041.53    9609.02   77759.09
    Latency        1.47ms     1.81ms   151.42ms
    HTTP codes:
      1xx - 0, 2xx - 95775, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4225
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4225
    Throughput:     7.45MB/s
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
    Reqs/sec     78256.35    4205.42   89530.06
    Latency      636.65us   183.49us     7.82ms
    HTTP codes:
      1xx - 0, 2xx - 92750, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7250
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7250
    Throughput:    11.49MB/s
  ```


