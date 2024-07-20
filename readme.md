## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `76846` | `6904` | `94516` |
| **84%** | [Hyper Express](#hyper-express) | `64621` | `3833` | `70418` |
| **34%** | [Node (Default)](#node-default) | `26084` | `8059` | `55008` |
| **31%** | [Fastify](#fastify) | `23672` | `7274` | `36516` |
| **27%** | [Hono](#hono) | `20710` | `5785` | `29847` |
| **24%** | [Koa](#koa) | `18683` | `6409` | `55266` |
| **11%** | [Carbon](#carbon) | `8252` | `1414` | `10365` |
| **8%** | [Express](#express) | `6506` | `1068` | `8473` |


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
    Reqs/sec      8550.28    4176.79   55199.53
    Latency        5.84ms     4.15ms   358.59ms
    HTTP codes:
      1xx - 0, 2xx - 94282, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5718
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5718
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
    Reqs/sec      6650.96    1082.42    8451.83
    Latency        7.51ms     3.57ms   340.77ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.90MB/s
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
    Reqs/sec     26130.29    8673.61   37385.62
    Latency        1.91ms     1.98ms   179.42ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.94MB/s
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
    Reqs/sec     20426.32    5746.30   30342.56
    Latency        2.45ms     1.92ms   174.03ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.61MB/s
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
    Reqs/sec     64601.55    4053.10   78373.19
    Latency      772.23us    69.17us     3.12ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.17MB/s
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
    Reqs/sec     18249.29    7410.89   58156.35
    Latency        2.73ms     2.52ms   217.68ms
    HTTP codes:
      1xx - 0, 2xx - 92183, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7817
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7817
    Throughput:     3.80MB/s
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
    Reqs/sec     25368.76    7749.18   45268.52
    Latency        1.97ms     1.91ms   162.07ms
    HTTP codes:
      1xx - 0, 2xx - 98192, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1808
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1808
    Throughput:     5.70MB/s
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
    Reqs/sec     76213.78    7243.97  109959.77
    Latency      658.13us   179.33us     7.95ms
    HTTP codes:
      1xx - 0, 2xx - 97649, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2351
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2351
    Throughput:    11.70MB/s
  ```


