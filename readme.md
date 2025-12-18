## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75437` | `2603` | `82225` |
| **83%** | [Hyper Express](#hyper-express) | `62650` | `3770` | `66347` |
| **36%** | [Node (Default)](#node-default) | `26837` | `8686` | `80508` |
| **32%** | [Fastify](#fastify) | `24205` | `7819` | `36866` |
| **28%** | [Hono](#hono) | `21112` | `6058` | `31021` |
| **25%** | [Koa](#koa) | `18867` | `7940` | `67070` |
| **11%** | [Carbon](#carbon) | `8329` | `1419` | `10504` |
| **9%** | [Express](#express) | `6572` | `1119` | `8389` |


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
    Reqs/sec      8788.31    5639.23   68135.19
    Latency        5.68ms     4.30ms   371.48ms
    HTTP codes:
      1xx - 0, 2xx - 92534, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7466
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7466
    Throughput:     1.85MB/s
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
    Reqs/sec      7105.25    5886.07   78872.59
    Latency        7.03ms     3.98ms   365.38ms
    HTTP codes:
      1xx - 0, 2xx - 90430, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9570
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9570
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
    Reqs/sec     24635.49    7993.72   36444.10
    Latency        2.03ms     2.04ms   182.99ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.58MB/s
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
    Reqs/sec     20928.28    5808.57   29278.48
    Latency        2.39ms     2.08ms   186.26ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.73MB/s
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
    Reqs/sec     62731.28    3256.20   66870.88
    Latency      795.07us    67.73us     3.91ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.91MB/s
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
    Reqs/sec     19788.78    8994.90   72294.47
    Latency        2.52ms     2.38ms   209.56ms
    HTTP codes:
      1xx - 0, 2xx - 92227, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7773
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7773
    Throughput:     4.13MB/s
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
    Reqs/sec     24620.07    6881.96   62484.54
    Latency        2.03ms     1.86ms   162.59ms
    HTTP codes:
      1xx - 0, 2xx - 97463, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2537
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2537
    Throughput:     5.50MB/s
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
    Reqs/sec     75330.47    2396.91   83055.51
    Latency      661.45us   126.63us     7.86ms
    HTTP codes:
      1xx - 0, 2xx - 96959, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3041
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3041
    Throughput:    11.56MB/s
  ```


