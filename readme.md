## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `144975` | `11995` | `162863` |
| **85%** | [Hyper Express](#hyper-express) | `123622` | `7791` | `138553` |
| **35%** | [Node (Default)](#node-default) | `50739` | `12624` | `125958` |
| **33%** | [Fastify](#fastify) | `47729` | `9234` | `56970` |
| **28%** | [Hono](#hono) | `41153` | `8642` | `65007` |
| **28%** | [Koa](#koa) | `40380` | `20670` | `143080` |
| **12%** | [Carbon](#carbon) | `18020` | `4887` | `28588` |
| **8%** | [Express](#express) | `12155` | `2592` | `18742` |


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
    Reqs/sec     20247.24   14275.92  126286.19
    Latency        2.46ms     3.23ms   268.08ms
    HTTP codes:
      1xx - 0, 2xx - 89390, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10610
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10610
    Throughput:     4.11MB/s
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
    Reqs/sec     14101.20   13945.55  138876.07
    Latency        3.54ms     2.67ms   237.01ms
    HTTP codes:
      1xx - 0, 2xx - 85700, 3xx - 0, 4xx - 0, 5xx - 0
      others - 14300
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 14300
    Throughput:     3.46MB/s
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
    Reqs/sec     57044.40   25264.90  128651.09
    Latency        0.87ms     1.23ms    97.15ms
    HTTP codes:
      1xx - 0, 2xx - 76072, 3xx - 0, 4xx - 0, 5xx - 0
      others - 23928
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 23927
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 1
    Throughput:     9.87MB/s
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
    Reqs/sec     47011.26   21277.52  133640.89
    Latency        1.06ms     1.22ms    95.08ms
    HTTP codes:
      1xx - 0, 2xx - 83803, 3xx - 0, 4xx - 0, 5xx - 0
      others - 16197
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 16197
    Throughput:     8.92MB/s
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
    Reqs/sec    127054.30    8717.94  138724.99
    Latency      390.69us   188.27us     8.98ms
    HTTP codes:
      1xx - 0, 2xx - 89666, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10334
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10334
    Throughput:    16.18MB/s
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
    Reqs/sec     41926.90   20353.43  135646.15
    Latency        1.19ms     1.20ms    97.61ms
    HTTP codes:
      1xx - 0, 2xx - 87120, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12880
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12880
    Throughput:     8.27MB/s
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
    Reqs/sec     52712.08   14086.11  135381.97
    Latency        0.94ms     0.94ms    75.17ms
    HTTP codes:
      1xx - 0, 2xx - 94936, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5064
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5064
    Throughput:    11.47MB/s
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
    Reqs/sec    146910.46    7485.07  160022.45
    Latency      338.04us   156.80us     5.44ms
    HTTP codes:
      1xx - 0, 2xx - 92477, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7523
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7523
    Throughput:    21.51MB/s
  ```


