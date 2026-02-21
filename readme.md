## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72797` | `2454` | `83338` |
| **82%** | [Hyper Express](#hyper-express) | `59331` | `3164` | `61952` |
| **32%** | [Node (Default)](#node-default) | `23295` | `7022` | `53253` |
| **30%** | [Fastify](#fastify) | `22142` | `7156` | `36827` |
| **27%** | [Hono](#hono) | `19862` | `5987` | `29411` |
| **25%** | [Koa](#koa) | `17977` | `8280` | `74817` |
| **11%** | [Carbon](#carbon) | `7885` | `1434` | `10332` |
| **8%** | [Express](#express) | `6141` | `1077` | `8168` |


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
    Reqs/sec      8291.49    5990.94   76872.72
    Latency        6.02ms     4.43ms   382.54ms
    HTTP codes:
      1xx - 0, 2xx - 90948, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9052
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9052
    Throughput:     1.71MB/s
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
    Reqs/sec      6202.83    1109.68    8314.38
    Latency        8.06ms     3.72ms   358.99ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.77MB/s
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
    Reqs/sec     22477.89    7601.97   36674.12
    Latency        2.22ms     2.10ms   190.94ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.10MB/s
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
    Reqs/sec     19421.04    5718.64   29070.34
    Latency        2.57ms     2.09ms   186.82ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.39MB/s
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
    Reqs/sec     59408.05    3240.16   64525.98
    Latency      840.16us    74.28us     3.36ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.43MB/s
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
    Reqs/sec     18282.84    8272.63   71302.55
    Latency        2.73ms     2.57ms   225.90ms
    HTTP codes:
      1xx - 0, 2xx - 92638, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7362
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7362
    Throughput:     3.83MB/s
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
    Reqs/sec     24101.50    8251.19   70053.97
    Latency        2.07ms     2.05ms   175.12ms
    HTTP codes:
      1xx - 0, 2xx - 96439, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3561
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3561
    Throughput:     5.33MB/s
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
    Reqs/sec     72660.03    2675.16   83384.40
    Latency      685.02us   184.51us    12.31ms
    HTTP codes:
      1xx - 0, 2xx - 96565, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3435
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3435
    Throughput:    11.11MB/s
  ```


