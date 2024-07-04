## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74928` | `6343` | `82863` |
| **85%** | [Hyper Express](#hyper-express) | `63474` | `3610` | `69916` |
| **34%** | [Node (Default)](#node-default) | `25501` | `8057` | `56003` |
| **32%** | [Fastify](#fastify) | `23866` | `7125` | `36814` |
| **28%** | [Hono](#hono) | `20650` | `5693` | `29654` |
| **24%** | [Koa](#koa) | `17815` | `6000` | `45293` |
| **11%** | [Carbon](#carbon) | `8043` | `1334` | `10329` |
| **9%** | [Express](#express) | `6471` | `1018` | `8511` |


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
    Reqs/sec      8905.82    5311.21   65864.02
    Latency        5.60ms     4.22ms   365.91ms
    HTTP codes:
      1xx - 0, 2xx - 91481, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8519
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8519
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
    Reqs/sec      6334.02     953.56    8431.90
    Latency        7.89ms     3.57ms   341.50ms
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
    Reqs/sec     24046.42    8066.77   37047.80
    Latency        2.08ms     2.06ms   183.77ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.45MB/s
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
    Reqs/sec     20894.26    6007.05   30151.32
    Latency        2.39ms     2.00ms   180.10ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.72MB/s
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
    Reqs/sec     63554.44    3838.84   69108.17
    Latency      784.62us    93.20us     4.87ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.03MB/s
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
    Reqs/sec     17672.00    5735.77   44335.25
    Latency        2.82ms     2.43ms   216.31ms
    HTTP codes:
      1xx - 0, 2xx - 95414, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4586
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4586
    Throughput:     3.82MB/s
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
    Reqs/sec     25414.19    7160.58   48773.81
    Latency        1.96ms     1.83ms   159.44ms
    HTTP codes:
      1xx - 0, 2xx - 97721, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2279
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2257
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 22
    Throughput:     5.69MB/s
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
    Reqs/sec     74466.30    7507.27   95125.25
    Latency      670.19us   210.52us    13.37ms
    HTTP codes:
      1xx - 0, 2xx - 97887, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2113
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2113
    Throughput:    11.52MB/s
  ```


