## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74471` | `5286` | `81853` |
| **84%** | [Hyper Express](#hyper-express) | `62535` | `3867` | `75642` |
| **38%** | [Node (Default)](#node-default) | `27954` | `8910` | `53147` |
| **34%** | [Fastify](#fastify) | `24981` | `7548` | `37381` |
| **27%** | [Hono](#hono) | `20340` | `5759` | `29621` |
| **25%** | [Koa](#koa) | `18381` | `6706` | `52001` |
| **11%** | [Carbon](#carbon) | `8335` | `1423` | `10350` |
| **9%** | [Express](#express) | `6425` | `1002` | `8488` |


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
    Reqs/sec      8757.81    4598.17   59417.44
    Latency        5.70ms     4.43ms   377.86ms
    HTTP codes:
      1xx - 0, 2xx - 93294, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6706
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6706
    Throughput:     1.86MB/s
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
    Reqs/sec      6988.33    4536.08   49560.16
    Latency        7.14ms     3.97ms   363.19ms
    HTTP codes:
      1xx - 0, 2xx - 91622, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8378
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8378
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
    Reqs/sec     25033.92    7731.42   35533.90
    Latency        2.00ms     2.08ms   184.16ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.68MB/s
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
    Reqs/sec     21530.50    5796.33   29828.73
    Latency        2.32ms     2.05ms   181.99ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.86MB/s
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
    Reqs/sec     61754.74    3809.43   69906.89
    Latency      807.42us    83.50us     3.33ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.77MB/s
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
    Reqs/sec     19605.48    7904.84   64564.51
    Latency        2.54ms     2.40ms   210.74ms
    HTTP codes:
      1xx - 0, 2xx - 91562, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8438
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8438
    Throughput:     4.06MB/s
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
    Reqs/sec     26599.70    8500.06   62301.20
    Latency        1.88ms     1.98ms   166.58ms
    HTTP codes:
      1xx - 0, 2xx - 96374, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3626
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3626
    Throughput:     5.86MB/s
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
    Reqs/sec     73331.32    5447.40   83836.43
    Latency      680.56us   174.77us     9.12ms
    HTTP codes:
      1xx - 0, 2xx - 97426, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2574
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2574
    Throughput:    11.29MB/s
  ```


