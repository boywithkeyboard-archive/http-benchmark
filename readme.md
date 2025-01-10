## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74173` | `4565` | `93705` |
| **84%** | [Hyper Express](#hyper-express) | `62551` | `3751` | `65907` |
| **32%** | [Node (Default)](#node-default) | `23828` | `6942` | `60043` |
| **29%** | [Fastify](#fastify) | `21313` | `4900` | `34822` |
| **25%** | [Hono](#hono) | `18531` | `4010` | `28968` |
| **22%** | [Koa](#koa) | `16125` | `4993` | `53955` |
| **11%** | [Carbon](#carbon) | `8286` | `1467` | `10521` |
| **9%** | [Express](#express) | `6492` | `1059` | `8511` |


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
    Reqs/sec      8737.04    4795.43   60522.66
    Latency        5.70ms     4.21ms   362.85ms
    HTTP codes:
      1xx - 0, 2xx - 92579, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7421
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7421
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
    Reqs/sec      6594.04    1103.44    8488.26
    Latency        7.58ms     3.47ms   334.23ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.89MB/s
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
    Reqs/sec     21318.94    5494.01   35090.97
    Latency        2.34ms     1.89ms   172.93ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.84MB/s
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
    Reqs/sec     18381.42    4030.22   29293.92
    Latency        2.72ms     2.04ms   185.11ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.15MB/s
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
    Reqs/sec     62288.32    3790.38   65768.34
    Latency      800.83us    71.11us     3.85ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.84MB/s
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
    Reqs/sec     16833.46    5149.18   51911.93
    Latency        2.96ms     2.35ms   204.66ms
    HTTP codes:
      1xx - 0, 2xx - 94895, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5105
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5105
    Throughput:     3.62MB/s
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
    Reqs/sec     22710.05    4777.78   50301.27
    Latency        2.18ms     1.79ms   160.78ms
    HTTP codes:
      1xx - 0, 2xx - 97114, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2886
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2886
    Throughput:     5.08MB/s
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
    Reqs/sec     74408.89    6485.83   79795.48
    Latency      668.17us   158.31us     9.55ms
    HTTP codes:
      1xx - 0, 2xx - 98411, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1589
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1589
    Throughput:    11.59MB/s
  ```


