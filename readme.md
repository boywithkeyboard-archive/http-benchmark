## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75279` | `6160` | `87184` |
| **83%** | [Hyper Express](#hyper-express) | `62558` | `4609` | `66576` |
| **37%** | [Node (Default)](#node-default) | `27495` | `8233` | `46709` |
| **32%** | [Fastify](#fastify) | `24349` | `6932` | `37224` |
| **29%** | [Hono](#hono) | `21537` | `6123` | `30151` |
| **26%** | [Koa](#koa) | `19303` | `7168` | `61367` |
| **11%** | [Carbon](#carbon) | `8376` | `1424` | `10411` |
| **9%** | [Express](#express) | `6436` | `1019` | `8386` |


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
    Reqs/sec      8877.61    4714.62   56093.96
    Latency        5.62ms     4.30ms   370.27ms
    HTTP codes:
      1xx - 0, 2xx - 93600, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6400
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6400
    Throughput:     1.89MB/s
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
    Reqs/sec      6823.74    4394.77   55814.57
    Latency        7.32ms     3.85ms   351.99ms
    HTTP codes:
      1xx - 0, 2xx - 92342, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7658
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7658
    Throughput:     1.80MB/s
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
    Reqs/sec     24838.48    7605.37   36844.25
    Latency        2.01ms     2.01ms   178.78ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.64MB/s
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
    Reqs/sec     21959.56    6045.37   29817.91
    Latency        2.28ms     2.05ms   183.50ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.96MB/s
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
    Reqs/sec     64396.70    3722.78   71568.35
    Latency      774.71us    69.02us     2.92ms
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
    Reqs/sec     19976.84    7762.18   56147.84
    Latency        2.50ms     2.41ms   213.75ms
    HTTP codes:
      1xx - 0, 2xx - 92030, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7970
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7970
    Throughput:     4.16MB/s
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
    Reqs/sec     26528.70    7363.66   52881.14
    Latency        1.88ms     1.88ms   160.99ms
    HTTP codes:
      1xx - 0, 2xx - 97462, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2538
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2538
    Throughput:     5.92MB/s
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
    Reqs/sec     73720.80    4946.23   80631.34
    Latency      676.19us   144.97us     6.86ms
    HTTP codes:
      1xx - 0, 2xx - 97860, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2140
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2140
    Throughput:    11.42MB/s
  ```


