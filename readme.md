## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72090` | `4495` | `76314` |
| **86%** | [Hyper Express](#hyper-express) | `61735` | `3399` | `66436` |
| **37%** | [Node (Default)](#node-default) | `26360` | `8076` | `57425` |
| **34%** | [Fastify](#fastify) | `24265` | `7448` | `36397` |
| **28%** | [Hono](#hono) | `20176` | `5317` | `29762` |
| **26%** | [Koa](#koa) | `19071` | `7384` | `59021` |
| **11%** | [Carbon](#carbon) | `8280` | `1431` | `10411` |
| **9%** | [Express](#express) | `6445` | `1067` | `8433` |


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
    Reqs/sec      8925.76    5233.02   62691.52
    Latency        5.59ms     4.39ms   379.80ms
    HTTP codes:
      1xx - 0, 2xx - 91407, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8593
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8593
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
    Reqs/sec      6886.83    4348.99   53111.68
    Latency        7.25ms     3.96ms   363.45ms
    HTTP codes:
      1xx - 0, 2xx - 92302, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7698
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7698
    Throughput:     1.82MB/s
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
    Reqs/sec     23711.05    6630.51   36606.09
    Latency        2.11ms     1.87ms   168.99ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.38MB/s
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
    Reqs/sec     21122.71    6048.57   29630.47
    Latency        2.37ms     2.03ms   181.80ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.77MB/s
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
    Reqs/sec     61053.18    3584.18   64119.23
    Latency      816.72us    77.78us     3.07ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.67MB/s
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
    Reqs/sec     19099.74    7316.25   53376.82
    Latency        2.61ms     2.38ms   210.15ms
    HTTP codes:
      1xx - 0, 2xx - 94010, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5990
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5990
    Throughput:     4.06MB/s
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
    Reqs/sec     26009.82    7249.25   45535.09
    Latency        1.92ms     1.90ms   164.79ms
    HTTP codes:
      1xx - 0, 2xx - 98137, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1863
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1863
    Throughput:     5.85MB/s
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
    Reqs/sec     72240.44    4266.28   80799.81
    Latency      688.93us   164.65us     7.01ms
    HTTP codes:
      1xx - 0, 2xx - 96830, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3170
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3170
    Throughput:    11.08MB/s
  ```


