## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `81492` | `3353` | `88562` |
| **85%** | [Hyper Express](#hyper-express) | `68874` | `3641` | `72489` |
| **45%** | [Node (Default)](#node-default) | `36330` | `8867` | `72576` |
| **42%** | [Fastify](#fastify) | `34579` | `10166` | `52573` |
| **36%** | [Koa](#koa) | `29366` | `13784` | `87667` |
| **36%** | [Hono](#hono) | `28941` | `8284` | `47214` |
| **11%** | [Carbon](#carbon) | `9105` | `2127` | `13316` |
| **10%** | [Express](#express) | `7755` | `1806` | `10801` |


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
    Reqs/sec     10165.14    7332.87   76325.29
    Latency        4.91ms     4.36ms   375.32ms
    HTTP codes:
      1xx - 0, 2xx - 90444, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9556
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9556
    Throughput:     2.09MB/s
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
    Reqs/sec      8317.42    6577.09   75128.05
    Latency        6.00ms     3.60ms   329.37ms
    HTTP codes:
      1xx - 0, 2xx - 90562, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9438
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9438
    Throughput:     2.16MB/s
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
    Reqs/sec     32686.66    8548.87   52150.02
    Latency        1.53ms     1.91ms   169.25ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.41MB/s
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
    Reqs/sec     30363.25    8793.37   46568.75
    Latency        1.65ms     2.08ms   181.92ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     6.86MB/s
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
    Reqs/sec     71988.88    4241.27   77940.35
    Latency      693.12us    82.09us     4.68ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    10.22MB/s
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
    Reqs/sec     31906.71   16113.72   91683.74
    Latency        1.56ms     2.26ms   196.41ms
    HTTP codes:
      1xx - 0, 2xx - 84965, 3xx - 0, 4xx - 0, 5xx - 0
      others - 15035
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 15035
    Throughput:     6.13MB/s
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
    Reqs/sec     36025.43    9767.97   78889.50
    Latency        1.38ms     1.74ms   146.05ms
    HTTP codes:
      1xx - 0, 2xx - 95753, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4247
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4247
    Throughput:     7.90MB/s
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
    Reqs/sec     79202.80    3263.65   86514.12
    Latency      629.96us   117.23us     4.33ms
    HTTP codes:
      1xx - 0, 2xx - 97199, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2801
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2801
    Throughput:    12.17MB/s
  ```


