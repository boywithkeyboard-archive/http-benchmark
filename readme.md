## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `82892` | `3225` | `89911` |
| **85%** | [Hyper Express](#hyper-express) | `70126` | `3391` | `73127` |
| **42%** | [Node (Default)](#node-default) | `34624` | `9566` | `76453` |
| **41%** | [Fastify](#fastify) | `34390` | `11094` | `53675` |
| **36%** | [Hono](#hono) | `30223` | `9074` | `44837` |
| **35%** | [Koa](#koa) | `28720` | `13333` | `82958` |
| **11%** | [Carbon](#carbon) | `9196` | `2194` | `13491` |
| **9%** | [Express](#express) | `7620` | `1749` | `11045` |


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
    Reqs/sec     10424.47    7341.30   90230.43
    Latency        4.78ms     4.15ms   360.13ms
    HTTP codes:
      1xx - 0, 2xx - 91398, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8602
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8602
    Throughput:     2.17MB/s
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
    Reqs/sec      8342.14    6941.52   76472.58
    Latency        5.98ms     3.64ms   332.54ms
    HTTP codes:
      1xx - 0, 2xx - 89834, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10166
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10166
    Throughput:     2.15MB/s
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
    Reqs/sec     33601.62   10086.76   52892.99
    Latency        1.49ms     1.96ms   171.57ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.60MB/s
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
    Reqs/sec     30804.47    9295.22   47156.70
    Latency        1.62ms     2.03ms   176.15ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     6.96MB/s
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
    Reqs/sec     69620.43    2869.37   72249.22
    Latency      716.32us    61.96us     2.59ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.89MB/s
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
    Reqs/sec     30845.88   13948.18   79143.16
    Latency        1.62ms     2.20ms   189.61ms
    HTTP codes:
      1xx - 0, 2xx - 90939, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9061
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9061
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
    Reqs/sec     34971.18    8986.38   74394.87
    Latency        1.43ms     1.76ms   152.23ms
    HTTP codes:
      1xx - 0, 2xx - 96340, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3660
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3660
    Throughput:     7.71MB/s
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
    Reqs/sec     81090.89    3221.76   86572.82
    Latency      613.69us   179.80us    11.10ms
    HTTP codes:
      1xx - 0, 2xx - 96046, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3954
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3954
    Throughput:    12.33MB/s
  ```


