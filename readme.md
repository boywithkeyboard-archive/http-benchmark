## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `82647` | `3189` | `92164` |
| **85%** | [Hyper Express](#hyper-express) | `70644` | `3460` | `73179` |
| **42%** | [Node (Default)](#node-default) | `34344` | `9464` | `75458` |
| **39%** | [Fastify](#fastify) | `32215` | `9252` | `52333` |
| **35%** | [Hono](#hono) | `29190` | `8259` | `48130` |
| **34%** | [Koa](#koa) | `28417` | `13399` | `87339` |
| **11%** | [Carbon](#carbon) | `9347` | `2306` | `13269` |
| **9%** | [Express](#express) | `7423` | `1734` | `10249` |


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
    Reqs/sec     10324.30    6965.50   84114.24
    Latency        4.84ms     4.20ms   364.63ms
    HTTP codes:
      1xx - 0, 2xx - 91462, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8538
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8538
    Throughput:     2.14MB/s
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
    Reqs/sec      7893.86    7293.74   79554.60
    Latency        6.32ms     3.88ms   355.96ms
    HTTP codes:
      1xx - 0, 2xx - 88666, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11334
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11334
    Throughput:     2.00MB/s
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
    Reqs/sec     33178.22    9768.34   54530.64
    Latency        1.51ms     1.82ms   162.52ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.50MB/s
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
    Reqs/sec     31238.98   10196.63   48165.83
    Latency        1.60ms     1.95ms   174.14ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.05MB/s
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
    Reqs/sec     71145.70    3175.80   76384.06
    Latency      701.33us    63.44us     2.83ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    10.10MB/s
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
    Reqs/sec     28778.98   14658.67   94239.18
    Latency        1.74ms     2.39ms   204.38ms
    HTTP codes:
      1xx - 0, 2xx - 88794, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11206
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11206
    Throughput:     5.76MB/s
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
    Reqs/sec     34422.69    9251.40   77404.67
    Latency        1.45ms     1.89ms   155.77ms
    HTTP codes:
      1xx - 0, 2xx - 96653, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3347
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3347
    Throughput:     7.62MB/s
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
    Reqs/sec     81588.67    4013.06   87622.27
    Latency      610.43us   142.09us     6.27ms
    HTTP codes:
      1xx - 0, 2xx - 97004, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2996
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2996
    Throughput:    12.53MB/s
  ```


