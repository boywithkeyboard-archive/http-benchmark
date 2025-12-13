## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75311` | `2747` | `84476` |
| **84%** | [Hyper Express](#hyper-express) | `63365` | `3547` | `67019` |
| **36%** | [Node (Default)](#node-default) | `26985` | `8934` | `66861` |
| **30%** | [Fastify](#fastify) | `22778` | `6871` | `36240` |
| **29%** | [Hono](#hono) | `22050` | `6074` | `31707` |
| **26%** | [Koa](#koa) | `19460` | `8071` | `68259` |
| **11%** | [Carbon](#carbon) | `8387` | `1484` | `10474` |
| **9%** | [Express](#express) | `6494` | `1105` | `8340` |


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
    Reqs/sec      9037.06    7156.82   77797.33
    Latency        5.52ms     4.30ms   369.69ms
    HTTP codes:
      1xx - 0, 2xx - 88694, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11306
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11306
    Throughput:     1.82MB/s
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
    Reqs/sec      7008.22    5222.41   66400.17
    Latency        7.12ms     3.93ms   359.45ms
    HTTP codes:
      1xx - 0, 2xx - 91592, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8408
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8408
    Throughput:     1.84MB/s
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
    Reqs/sec     23292.36    6823.50   36115.26
    Latency        2.15ms     2.10ms   187.72ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.28MB/s
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
    Reqs/sec     20954.47    5946.41   30211.31
    Latency        2.38ms     2.06ms   187.13ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.74MB/s
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
    Reqs/sec     63410.19    3804.86   66547.74
    Latency      786.35us    75.18us     2.96ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.01MB/s
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
    Reqs/sec     19634.12    8943.46   76338.61
    Latency        2.54ms     2.43ms   219.53ms
    HTTP codes:
      1xx - 0, 2xx - 92009, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7991
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7991
    Throughput:     4.09MB/s
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
    Reqs/sec     25692.77    7848.25   64007.88
    Latency        1.94ms     1.98ms   170.72ms
    HTTP codes:
      1xx - 0, 2xx - 97376, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2624
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2624
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
    Reqs/sec     75207.21    3143.58   80958.74
    Latency      661.70us   174.30us    10.69ms
    HTTP codes:
      1xx - 0, 2xx - 96624, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3376
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3376
    Throughput:    11.50MB/s
  ```


