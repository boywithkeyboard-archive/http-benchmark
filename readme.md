## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74989` | `3406` | `84951` |
| **82%** | [Hyper Express](#hyper-express) | `61738` | `3702` | `65248` |
| **33%** | [Node (Default)](#node-default) | `24849` | `7780` | `65294` |
| **31%** | [Fastify](#fastify) | `23510` | `6937` | `35430` |
| **28%** | [Hono](#hono) | `20953` | `6188` | `30031` |
| **25%** | [Koa](#koa) | `18848` | `7970` | `66135` |
| **11%** | [Carbon](#carbon) | `8160` | `1403` | `10292` |
| **9%** | [Express](#express) | `6384` | `1042` | `8430` |


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
    Reqs/sec      9053.41    6915.78   75792.65
    Latency        5.51ms     4.25ms   369.82ms
    HTTP codes:
      1xx - 0, 2xx - 89581, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10419
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10419
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
    Reqs/sec      6458.71    1085.36    8428.65
    Latency        7.74ms     3.71ms   355.53ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.85MB/s
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
    Reqs/sec     24184.89    7291.66   34518.57
    Latency        2.07ms     2.00ms   178.50ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.49MB/s
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
    Reqs/sec     20434.05    5624.24   30142.31
    Latency        2.44ms     2.13ms   189.86ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.62MB/s
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
    Reqs/sec     62243.14    3390.97   65907.32
    Latency      800.61us    69.97us     2.98ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.85MB/s
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
    Reqs/sec     18901.36    8339.56   71553.79
    Latency        2.64ms     2.42ms   213.91ms
    HTTP codes:
      1xx - 0, 2xx - 92527, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7473
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7473
    Throughput:     3.96MB/s
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
    Reqs/sec     25502.16    8326.77   77037.11
    Latency        1.96ms     1.94ms   173.87ms
    HTTP codes:
      1xx - 0, 2xx - 96522, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3478
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3478
    Throughput:     5.64MB/s
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
    Reqs/sec     73679.73    3065.69   81361.52
    Latency      676.12us   149.48us     7.23ms
    HTTP codes:
      1xx - 0, 2xx - 97296, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2704
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2704
    Throughput:    11.34MB/s
  ```


