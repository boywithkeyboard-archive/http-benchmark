## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `71201` | `4086` | `84371` |
| **83%** | [Hyper Express](#hyper-express) | `59360` | `3755` | `66702` |
| **30%** | [Node (Default)](#node-default) | `21278` | `6888` | `76802` |
| **29%** | [Fastify](#fastify) | `20949` | `5902` | `35046` |
| **29%** | [Hono](#hono) | `20940` | `6397` | `29628` |
| **26%** | [Koa](#koa) | `18274` | `8576` | `66846` |
| **10%** | [Carbon](#carbon) | `7456` | `1288` | `10310` |
| **9%** | [Express](#express) | `6151` | `1115` | `8177` |


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
    Reqs/sec      8156.26    5906.12   71176.42
    Latency        6.12ms     4.51ms   387.47ms
    HTTP codes:
      1xx - 0, 2xx - 91075, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8925
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8925
    Throughput:     1.69MB/s
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
    Reqs/sec      6110.12    1081.79    8215.68
    Latency        8.18ms     3.93ms   374.99ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.75MB/s
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
    Reqs/sec     21063.27    6260.46   35183.46
    Latency        2.37ms     2.20ms   194.95ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.78MB/s
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
    Reqs/sec     21534.70    6141.85   28937.76
    Latency        2.32ms     2.09ms   189.21ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.86MB/s
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
    Reqs/sec     59484.66    3358.17   67686.13
    Latency      839.43us    87.51us     5.50ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.44MB/s
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
    Reqs/sec     19285.38    8471.07   67709.28
    Latency        2.59ms     2.41ms   208.52ms
    HTTP codes:
      1xx - 0, 2xx - 93067, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6933
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6933
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
    Reqs/sec     21308.24    6314.91   63898.49
    Latency        2.34ms     2.12ms   179.05ms
    HTTP codes:
      1xx - 0, 2xx - 96877, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3123
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3123
    Throughput:     4.73MB/s
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
    Reqs/sec     70538.95    3974.84   83169.26
    Latency      705.35us   167.39us     8.54ms
    HTTP codes:
      1xx - 0, 2xx - 96834, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3166
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3166
    Throughput:    10.81MB/s
  ```


