## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `119305` | `6563` | `124584` |
| **100%** | [Hyper Express](#hyper-express) | `118826` | `7295` | `125470` |
| **64%** | [Node (Default)](#node-default) | `76557` | `16464` | `125046` |
| **59%** | [Fastify](#fastify) | `70531` | `20795` | `89138` |
| **54%** | [Koa](#koa) | `64439` | `19362` | `119257` |
| **53%** | [Hono](#hono) | `63739` | `15833` | `83212` |
| **22%** | [Carbon](#carbon) | `25655` | `7342` | `37552` |
| **16%** | [Express](#express) | `18789` | `3871` | `25160` |


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
    Reqs/sec     28975.57   16835.85  134575.31
    Latency        1.72ms     2.97ms   250.19ms
    HTTP codes:
      1xx - 0, 2xx - 87781, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12219
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12219
    Throughput:     5.78MB/s
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
    Reqs/sec     20190.33   10496.27  114043.11
    Latency        2.47ms     2.10ms   187.74ms
    HTTP codes:
      1xx - 0, 2xx - 92285, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7715
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7715
    Throughput:     5.33MB/s
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
    Reqs/sec     78414.60   22910.88  130152.69
    Latency      633.86us     0.96ms    76.05ms
    HTTP codes:
      1xx - 0, 2xx - 82188, 3xx - 0, 4xx - 0, 5xx - 0
      others - 17812
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 17812
    Throughput:    14.63MB/s
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
    Reqs/sec     71906.72   19582.28  118416.90
    Latency      693.65us   845.28us    67.20ms
    HTTP codes:
      1xx - 0, 2xx - 93124, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6876
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6876
    Throughput:    15.13MB/s
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
    Reqs/sec    124567.08   19838.50  174594.28
    Latency      398.70us   225.31us     8.17ms
    HTTP codes:
      1xx - 0, 2xx - 69945, 3xx - 0, 4xx - 0, 5xx - 0
      others - 30055
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 30055
    Throughput:    12.39MB/s
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
    Reqs/sec     64917.91   19861.50  126649.99
    Latency      767.31us     0.87ms    75.33ms
    HTTP codes:
      1xx - 0, 2xx - 89902, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10098
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10098
    Throughput:    13.18MB/s
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
    Reqs/sec     76359.90   17463.46  117541.74
    Latency      653.79us   834.43us    64.02ms
    HTTP codes:
      1xx - 0, 2xx - 96789, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3211
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3211
    Throughput:    16.90MB/s
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
    Reqs/sec    127310.19    5115.57  133395.19
    Latency      388.51us   112.39us     7.88ms
    HTTP codes:
      1xx - 0, 2xx - 96367, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3633
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3633
    Throughput:    19.50MB/s
  ```


