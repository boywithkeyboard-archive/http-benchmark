## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73653` | `5749` | `84531` |
| **85%** | [Hyper Express](#hyper-express) | `62936` | `4366` | `85945` |
| **34%** | [Node (Default)](#node-default) | `25299` | `7511` | `64713` |
| **33%** | [Fastify](#fastify) | `24118` | `7105` | `35583` |
| **27%** | [Hono](#hono) | `19878` | `5680` | `29155` |
| **24%** | [Koa](#koa) | `17986` | `6123` | `55062` |
| **11%** | [Carbon](#carbon) | `8296` | `1452` | `10428` |
| **9%** | [Express](#express) | `6514` | `1033` | `8406` |


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
    Reqs/sec      8562.06    3219.88   44188.72
    Latency        5.82ms     4.23ms   365.83ms
    HTTP codes:
      1xx - 0, 2xx - 95878, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4122
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4122
    Throughput:     1.87MB/s
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
    Reqs/sec      6486.18    1008.61    8448.64
    Latency        7.70ms     3.43ms   332.38ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.86MB/s
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
    Reqs/sec     25018.39    7707.65   36216.84
    Latency        2.00ms     1.96ms   177.86ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.68MB/s
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
    Reqs/sec     21429.31    5890.33   29161.44
    Latency        2.33ms     2.03ms   184.04ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.84MB/s
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
    Reqs/sec     62769.57    4741.62   74303.79
    Latency      794.02us    71.17us     3.25ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.92MB/s
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
    Reqs/sec     17733.40    6095.86   49760.89
    Latency        2.81ms     2.38ms   208.93ms
    HTTP codes:
      1xx - 0, 2xx - 94468, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5532
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5532
    Throughput:     3.79MB/s
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
    Reqs/sec     26527.47    7952.80   50702.08
    Latency        1.88ms     1.85ms   156.16ms
    HTTP codes:
      1xx - 0, 2xx - 97217, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2783
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2783
    Throughput:     5.91MB/s
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
    Reqs/sec     73292.15    5312.99   82929.50
    Latency      679.38us   171.50us     7.80ms
    HTTP codes:
      1xx - 0, 2xx - 97156, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2844
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2844
    Throughput:    11.28MB/s
  ```


