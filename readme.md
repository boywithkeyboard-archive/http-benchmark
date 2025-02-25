## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74114` | `4720` | `82436` |
| **83%** | [Hyper Express](#hyper-express) | `61776` | `4413` | `70204` |
| **35%** | [Node (Default)](#node-default) | `25948` | `7650` | `48352` |
| **35%** | [Fastify](#fastify) | `25870` | `7860` | `36901` |
| **29%** | [Hono](#hono) | `21287` | `5943` | `29913` |
| **26%** | [Koa](#koa) | `19295` | `6832` | `54380` |
| **11%** | [Carbon](#carbon) | `8371` | `1438` | `10433` |
| **9%** | [Express](#express) | `6473` | `1053` | `8518` |


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
    Reqs/sec      8579.10    3708.52   49594.74
    Latency        5.82ms     4.40ms   379.54ms
    HTTP codes:
      1xx - 0, 2xx - 95129, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4871
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4871
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
    Reqs/sec      6463.66    1069.78    8473.37
    Latency        7.73ms     3.54ms   338.03ms
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
    Reqs/sec     25672.78    8248.38   37251.02
    Latency        1.94ms     2.06ms   182.96ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.83MB/s
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
    Reqs/sec     21274.46    5607.50   30004.92
    Latency        2.35ms     2.05ms   184.49ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.80MB/s
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
    Reqs/sec     63661.36    3972.20   68762.90
    Latency      783.17us    68.53us     2.77ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.04MB/s
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
    Reqs/sec     18654.47    7216.32   61885.58
    Latency        2.68ms     2.30ms   200.54ms
    HTTP codes:
      1xx - 0, 2xx - 92096, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7904
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7904
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
    Reqs/sec     28039.78    8551.98   54269.52
    Latency        1.78ms     1.83ms   155.93ms
    HTTP codes:
      1xx - 0, 2xx - 97046, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2954
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2954
    Throughput:     6.23MB/s
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
    Reqs/sec     73671.61    5289.76   93455.83
    Latency      676.34us   204.90us     9.02ms
    HTTP codes:
      1xx - 0, 2xx - 97407, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2593
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2593
    Throughput:    11.36MB/s
  ```


