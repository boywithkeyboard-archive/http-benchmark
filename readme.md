## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `82468` | `2159` | `87844` |
| **87%** | [Hyper Express](#hyper-express) | `71738` | `3314` | `73642` |
| **46%** | [Node (Default)](#node-default) | `37898` | `11027` | `87476` |
| **45%** | [Fastify](#fastify) | `37372` | `11629` | `56350` |
| **36%** | [Hono](#hono) | `29813` | `7900` | `49879` |
| **35%** | [Koa](#koa) | `29108` | `13052` | `84918` |
| **11%** | [Carbon](#carbon) | `9436` | `2410` | `13804` |
| **9%** | [Express](#express) | `7510` | `1619` | `10432` |


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
    Reqs/sec     10748.49    9337.38   91884.55
    Latency        4.64ms     4.25ms   368.94ms
    HTTP codes:
      1xx - 0, 2xx - 87783, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12217
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12217
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
    Reqs/sec      8482.66    7293.70   83247.82
    Latency        5.89ms     3.81ms   346.88ms
    HTTP codes:
      1xx - 0, 2xx - 89709, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10291
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10291
    Throughput:     2.18MB/s
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
    Reqs/sec     37438.64   12660.30   55992.35
    Latency        1.33ms     1.93ms   167.99ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.49MB/s
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
    Reqs/sec     30069.46    7833.78   49561.07
    Latency        1.66ms     1.94ms   172.00ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     6.78MB/s
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
    Reqs/sec     71779.09    3325.65   75943.76
    Latency      694.73us    54.59us     2.50ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    10.20MB/s
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
    Reqs/sec     29616.90   12433.28   75777.87
    Latency        1.69ms     2.21ms   194.20ms
    HTTP codes:
      1xx - 0, 2xx - 93234, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6766
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6766
    Throughput:     6.23MB/s
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
    Reqs/sec     38984.08   12576.73   73035.28
    Latency        1.28ms     1.88ms   160.50ms
    HTTP codes:
      1xx - 0, 2xx - 97058, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2942
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2942
    Throughput:     8.65MB/s
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
    Reqs/sec     84379.50    2247.70   89625.06
    Latency      589.91us   164.12us    10.01ms
    HTTP codes:
      1xx - 0, 2xx - 95902, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4098
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4098
    Throughput:    12.81MB/s
  ```


