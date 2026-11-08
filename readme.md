## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `69661` | `3734` | `83126` |
| **82%** | [Hyper Express](#hyper-express) | `56803` | `2499` | `61770` |
| **34%** | [Node (Default)](#node-default) | `23959` | `7700` | `52702` |
| **34%** | [Fastify](#fastify) | `23920` | `7735` | `34075` |
| **28%** | [Hono](#hono) | `19550` | `6096` | `28632` |
| **25%** | [Koa](#koa) | `17202` | `8739` | `77236` |
| **11%** | [Carbon](#carbon) | `7330` | `1342` | `10037` |
| **8%** | [Express](#express) | `5857` | `996` | `8042` |


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
    Reqs/sec      8639.80    5459.80   65286.59
    Latency        5.76ms     4.62ms   390.37ms
    HTTP codes:
      1xx - 0, 2xx - 91956, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8044
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8044
    Throughput:     1.81MB/s
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
    Reqs/sec      6197.97    1092.53    8296.99
    Latency        8.06ms     3.80ms   368.06ms
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
    Reqs/sec     22638.49    7145.22   33873.61
    Latency        2.21ms     2.20ms   197.06ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.14MB/s
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
    Reqs/sec     18721.72    5678.77   27664.76
    Latency        2.67ms     2.49ms   218.90ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.23MB/s
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
    Reqs/sec     57209.35    3614.87   65048.72
    Latency        0.87ms    97.37us     3.92ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.13MB/s
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
    Reqs/sec     16871.70    7619.09   66557.26
    Latency        2.96ms     2.53ms   221.74ms
    HTTP codes:
      1xx - 0, 2xx - 92706, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7294
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7294
    Throughput:     3.54MB/s
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
    Reqs/sec     24750.75    8595.09   74422.71
    Latency        2.01ms     2.03ms   177.92ms
    HTTP codes:
      1xx - 0, 2xx - 95885, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4115
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4115
    Throughput:     5.44MB/s
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
    Reqs/sec     69865.08    4932.96   90261.96
    Latency      713.78us   166.18us     5.59ms
    HTTP codes:
      1xx - 0, 2xx - 97307, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2693
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2693
    Throughput:    10.75MB/s
  ```


