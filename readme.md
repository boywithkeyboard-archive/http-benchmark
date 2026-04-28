## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72947` | `4993` | `86087` |
| **82%** | [Hyper Express](#hyper-express) | `59548` | `3461` | `66029` |
| **29%** | [Hono](#hono) | `21473` | `6507` | `30448` |
| **29%** | [Fastify](#fastify) | `21228` | `5954` | `37274` |
| **27%** | [Node (Default)](#node-default) | `19714` | `5207` | `69221` |
| **26%** | [Koa](#koa) | `18980` | `9929` | `69742` |
| **10%** | [Carbon](#carbon) | `7316` | `1149` | `10152` |
| **8%** | [Express](#express) | `6174` | `1092` | `8281` |


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
    Reqs/sec      7856.52    5319.57   74320.51
    Latency        6.34ms     4.49ms   387.26ms
    HTTP codes:
      1xx - 0, 2xx - 92176, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7824
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7824
    Throughput:     1.65MB/s
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
    Reqs/sec      6151.91    1122.05    8268.20
    Latency        8.12ms     4.03ms   383.13ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.76MB/s
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
    Reqs/sec     19639.89    5381.82   36443.02
    Latency        2.54ms     2.14ms   193.06ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.46MB/s
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
    Reqs/sec     22125.15    6987.61   30577.42
    Latency        2.26ms     2.17ms   197.75ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.99MB/s
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
    Reqs/sec     59530.20    3534.19   65044.06
    Latency      837.44us    87.51us     3.24ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.46MB/s
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
    Reqs/sec     18800.05    8264.55   63466.25
    Latency        2.65ms     2.51ms   216.56ms
    HTTP codes:
      1xx - 0, 2xx - 93536, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6464
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6464
    Throughput:     3.98MB/s
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
    Reqs/sec     21054.11    5616.57   65045.83
    Latency        2.37ms     1.96ms   174.78ms
    HTTP codes:
      1xx - 0, 2xx - 96786, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3214
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3214
    Throughput:     4.66MB/s
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
    Reqs/sec     72063.57    4460.24   82731.26
    Latency      691.38us   151.12us     6.52ms
    HTTP codes:
      1xx - 0, 2xx - 97182, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2818
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2818
    Throughput:    11.08MB/s
  ```


