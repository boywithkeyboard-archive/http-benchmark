## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73129` | `4080` | `84853` |
| **84%** | [Hyper Express](#hyper-express) | `61650` | `3709` | `69962` |
| **32%** | [Hono](#hono) | `23080` | `6942` | `31407` |
| **31%** | [Node (Default)](#node-default) | `22349` | `5990` | `60609` |
| **29%** | [Fastify](#fastify) | `21529` | `5668` | `37652` |
| **26%** | [Koa](#koa) | `18994` | `9002` | `81025` |
| **10%** | [Carbon](#carbon) | `7523` | `1231` | `10236` |
| **9%** | [Express](#express) | `6236` | `1048` | `8228` |


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
    Reqs/sec      8117.70    5661.20   79079.97
    Latency        6.15ms     4.40ms   379.14ms
    HTTP codes:
      1xx - 0, 2xx - 91853, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8147
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8147
    Throughput:     1.69MB/s
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
    Reqs/sec      6327.99    1072.80    8220.59
    Latency        7.90ms     3.79ms   365.60ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.81MB/s
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
    Reqs/sec     20796.16    5544.53   37822.06
    Latency        2.40ms     2.09ms   185.52ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.71MB/s
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
    Reqs/sec     22166.78    6392.16   30653.66
    Latency        2.25ms     2.11ms   187.91ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.01MB/s
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
    Reqs/sec     61445.66    3599.31   66081.43
    Latency      811.38us    79.28us     2.99ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.73MB/s
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
    Reqs/sec     18742.60    9271.62   76274.90
    Latency        2.66ms     2.46ms   215.22ms
    HTTP codes:
      1xx - 0, 2xx - 92124, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7876
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7876
    Throughput:     3.91MB/s
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
    Reqs/sec     22824.18    7669.92   81470.54
    Latency        2.19ms     2.01ms   174.16ms
    HTTP codes:
      1xx - 0, 2xx - 95577, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4423
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4423
    Throughput:     5.00MB/s
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
    Reqs/sec     74480.77    4757.41   85380.29
    Latency      669.28us   188.17us    10.78ms
    HTTP codes:
      1xx - 0, 2xx - 96133, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3867
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3867
    Throughput:    11.32MB/s
  ```


