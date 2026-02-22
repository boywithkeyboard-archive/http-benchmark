## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72919` | `2527` | `79224` |
| **83%** | [Hyper Express](#hyper-express) | `60254` | `2806` | `63418` |
| **32%** | [Fastify](#fastify) | `23643` | `7634` | `35536` |
| **32%** | [Node (Default)](#node-default) | `23384` | `6982` | `66428` |
| **29%** | [Hono](#hono) | `20941` | `6489` | `29202` |
| **24%** | [Koa](#koa) | `17763` | `8079` | `71399` |
| **11%** | [Carbon](#carbon) | `8044` | `1488` | `10374` |
| **9%** | [Express](#express) | `6460` | `1162` | `8371` |


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
    Reqs/sec      8713.01    5535.72   66579.72
    Latency        5.73ms     4.31ms   371.87ms
    HTTP codes:
      1xx - 0, 2xx - 92527, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7473
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7473
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
    Reqs/sec      6392.50    1129.43    8349.05
    Latency        7.82ms     3.69ms   353.61ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.83MB/s
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
    Reqs/sec     23835.57    7943.99   37043.08
    Latency        2.10ms     1.99ms   180.26ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.41MB/s
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
    Reqs/sec     19760.03    5593.79   29827.30
    Latency        2.53ms     2.09ms   187.48ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.46MB/s
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
    Reqs/sec     58932.76    3163.67   61717.79
    Latency      846.06us    82.66us     4.56ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.37MB/s
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
    Reqs/sec     18340.49    9666.88   77369.65
    Latency        2.72ms     2.44ms   212.92ms
    HTTP codes:
      1xx - 0, 2xx - 90106, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9894
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9894
    Throughput:     3.74MB/s
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
    Reqs/sec     23255.22    6820.15   61910.81
    Latency        2.15ms     1.84ms   157.89ms
    HTTP codes:
      1xx - 0, 2xx - 97431, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2569
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2569
    Throughput:     5.19MB/s
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
    Reqs/sec     73840.64    2704.90   82464.84
    Latency      673.86us   182.74us    12.65ms
    HTTP codes:
      1xx - 0, 2xx - 95608, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4392
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4392
    Throughput:    11.17MB/s
  ```


