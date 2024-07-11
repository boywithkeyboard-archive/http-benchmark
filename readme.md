## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75117` | `7434` | `93443` |
| **85%** | [Hyper Express](#hyper-express) | `64135` | `3746` | `72994` |
| **35%** | [Node (Default)](#node-default) | `26023` | `8085` | `41589` |
| **31%** | [Fastify](#fastify) | `23617` | `7555` | `37598` |
| **27%** | [Hono](#hono) | `20594` | `6136` | `30853` |
| **24%** | [Koa](#koa) | `17766` | `5640` | `55141` |
| **11%** | [Carbon](#carbon) | `8222` | `1391` | `10358` |
| **9%** | [Express](#express) | `6523` | `1008` | `8576` |


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
    Reqs/sec      8392.72    3346.05   48377.28
    Latency        5.95ms     4.13ms   359.83ms
    HTTP codes:
      1xx - 0, 2xx - 95771, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4229
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4229
    Throughput:     1.83MB/s
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
    Reqs/sec      6662.56    1089.96    8569.65
    Latency        7.50ms     3.51ms   337.47ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.91MB/s
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
    Reqs/sec     24866.22    7730.95   37165.87
    Latency        2.01ms     2.02ms   181.38ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.63MB/s
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
    Reqs/sec     20595.41    5846.77   29981.68
    Latency        2.43ms     2.04ms   184.20ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.65MB/s
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
    Reqs/sec     64335.46    3726.65   68964.75
    Latency      775.26us    69.11us     3.02ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.14MB/s
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
    Reqs/sec     18230.75    6654.40   51854.93
    Latency        2.74ms     2.39ms   209.72ms
    HTTP codes:
      1xx - 0, 2xx - 94410, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5590
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5590
    Throughput:     3.89MB/s
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
    Reqs/sec     26089.16    8129.08   57655.53
    Latency        1.91ms     1.79ms   159.44ms
    HTTP codes:
      1xx - 0, 2xx - 97351, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2649
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2649
    Throughput:     5.82MB/s
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
    Reqs/sec     75449.03    7057.50   88512.66
    Latency      660.70us   189.49us     8.92ms
    HTTP codes:
      1xx - 0, 2xx - 98094, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1906
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1906
    Throughput:    11.71MB/s
  ```


