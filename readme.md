## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73761` | `7271` | `83384` |
| **87%** | [Hyper Express](#hyper-express) | `63857` | `3413` | `73929` |
| **35%** | [Node (Default)](#node-default) | `25875` | `6931` | `60101` |
| **33%** | [Fastify](#fastify) | `24051` | `6969` | `35864` |
| **27%** | [Hono](#hono) | `19640` | `5768` | `30507` |
| **24%** | [Koa](#koa) | `17679` | `5833` | `51064` |
| **11%** | [Carbon](#carbon) | `8157` | `1365` | `10370` |
| **9%** | [Express](#express) | `6526` | `1070` | `8424` |


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
    Reqs/sec      8722.63    4925.01   62487.32
    Latency        5.71ms     4.17ms   357.98ms
    HTTP codes:
      1xx - 0, 2xx - 92535, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7465
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7465
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
    Reqs/sec      6578.88    1108.43    8370.44
    Latency        7.60ms     3.48ms   337.02ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.88MB/s
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
    Reqs/sec     23772.56    7361.78   35792.57
    Latency        2.10ms     2.01ms   181.75ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.39MB/s
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
    Reqs/sec     20442.50    6083.80   30003.48
    Latency        2.44ms     2.04ms   181.77ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.62MB/s
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
    Reqs/sec     63733.91    3333.22   66566.61
    Latency      782.48us    62.88us     4.17ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.05MB/s
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
    Reqs/sec     18333.67    6742.48   59904.79
    Latency        2.72ms     2.36ms   207.76ms
    HTTP codes:
      1xx - 0, 2xx - 94080, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5920
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5920
    Throughput:     3.90MB/s
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
    Reqs/sec     25525.15    7225.44   43988.92
    Latency        1.95ms     1.83ms   157.78ms
    HTTP codes:
      1xx - 0, 2xx - 98243, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1757
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1757
    Throughput:     5.74MB/s
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
    Reqs/sec     73863.80    6273.12   82966.56
    Latency      674.78us   191.47us    12.43ms
    HTTP codes:
      1xx - 0, 2xx - 97326, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2674
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2674
    Throughput:    11.37MB/s
  ```


