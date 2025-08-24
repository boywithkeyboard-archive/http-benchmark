## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74751` | `2369` | `83598` |
| **84%** | [Hyper Express](#hyper-express) | `62903` | `3681` | `66065` |
| **35%** | [Node (Default)](#node-default) | `26165` | `8332` | `61303` |
| **32%** | [Fastify](#fastify) | `24135` | `7567` | `35182` |
| **27%** | [Hono](#hono) | `20174` | `5810` | `29740` |
| **25%** | [Koa](#koa) | `18595` | `8833` | `80334` |
| **11%** | [Carbon](#carbon) | `7989` | `1445` | `10204` |
| **8%** | [Express](#express) | `6327` | `1043` | `8251` |


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
    Reqs/sec      8675.55    6834.96   76955.72
    Latency        5.75ms     4.43ms   382.12ms
    HTTP codes:
      1xx - 0, 2xx - 89541, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10459
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10459
    Throughput:     1.77MB/s
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
    Reqs/sec      6171.50    1008.38    8158.83
    Latency        8.10ms     3.70ms   358.56ms
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
    Reqs/sec     23920.89    7598.62   34616.68
    Latency        2.09ms     2.08ms   186.32ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.42MB/s
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
    Reqs/sec     20865.99    6067.06   29287.79
    Latency        2.39ms     2.11ms   187.99ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.71MB/s
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
    Reqs/sec     62541.66    3079.45   65710.81
    Latency      797.20us    69.87us     3.06ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.88MB/s
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
    Reqs/sec     18574.05    8343.23   66555.08
    Latency        2.69ms     2.40ms   212.65ms
    HTTP codes:
      1xx - 0, 2xx - 92395, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7605
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7605
    Throughput:     3.88MB/s
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
    Reqs/sec     26414.51    9523.84   74836.60
    Latency        1.89ms     2.04ms   171.78ms
    HTTP codes:
      1xx - 0, 2xx - 95835, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4165
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4165
    Throughput:     5.79MB/s
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
    Reqs/sec     74247.58    2679.42   77330.81
    Latency      670.68us   185.06us    11.04ms
    HTTP codes:
      1xx - 0, 2xx - 96642, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3358
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3358
    Throughput:    11.36MB/s
  ```


