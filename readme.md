## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `82342` | `3830` | `86820` |
| **86%** | [Hyper Express](#hyper-express) | `70407` | `3240` | `73196` |
| **42%** | [Node (Default)](#node-default) | `34707` | `9696` | `81870` |
| **41%** | [Fastify](#fastify) | `34082` | `10557` | `52362` |
| **37%** | [Hono](#hono) | `30823` | `9924` | `48172` |
| **37%** | [Koa](#koa) | `30264` | `13165` | `87537` |
| **11%** | [Carbon](#carbon) | `9181` | `2179` | `13376` |
| **9%** | [Express](#express) | `7566` | `1794` | `10734` |


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
    Reqs/sec     10259.98    7044.02   84909.03
    Latency        4.86ms     4.27ms   368.40ms
    HTTP codes:
      1xx - 0, 2xx - 91192, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8808
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8808
    Throughput:     2.13MB/s
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
    Reqs/sec      8121.45    7051.09   79029.44
    Latency        6.14ms     3.82ms   348.71ms
    HTTP codes:
      1xx - 0, 2xx - 89276, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10724
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10724
    Throughput:     2.08MB/s
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
    Reqs/sec     31772.01    9280.28   51639.02
    Latency        1.57ms     1.95ms   168.55ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.21MB/s
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
    Reqs/sec     29121.01    7944.98   48896.09
    Latency        1.72ms     1.97ms   172.25ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     6.56MB/s
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
    Reqs/sec     70705.73    3317.84   74204.68
    Latency      705.35us    67.02us     3.22ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    10.04MB/s
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
    Reqs/sec     31246.26   14512.15   90982.82
    Latency        1.60ms     2.37ms   205.49ms
    HTTP codes:
      1xx - 0, 2xx - 90966, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9034
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9034
    Throughput:     6.43MB/s
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
    Reqs/sec     35199.64   11475.86   89454.79
    Latency        1.42ms     1.76ms   150.39ms
    HTTP codes:
      1xx - 0, 2xx - 94150, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5850
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5850
    Throughput:     7.59MB/s
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
    Reqs/sec     81827.25    3394.23   87419.71
    Latency      608.99us   193.67us     9.09ms
    HTTP codes:
      1xx - 0, 2xx - 96500, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3500
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3500
    Throughput:    12.50MB/s
  ```


