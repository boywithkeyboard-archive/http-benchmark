## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `70060` | `3898` | `81462` |
| **82%** | [Hyper Express](#hyper-express) | `57746` | `3422` | `65094` |
| **30%** | [Hono](#hono) | `21038` | `7026` | `30808` |
| **29%** | [Node (Default)](#node-default) | `20652` | `6012` | `68196` |
| **29%** | [Fastify](#fastify) | `20512` | `5699` | `35882` |
| **28%** | [Koa](#koa) | `19545` | `8682` | `65358` |
| **10%** | [Carbon](#carbon) | `7325` | `1326` | `10289` |
| **9%** | [Express](#express) | `6085` | `1061` | `8344` |


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
    Reqs/sec      7920.79    6035.28   65996.47
    Latency        6.30ms     4.61ms   387.62ms
    HTTP codes:
      1xx - 0, 2xx - 89675, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10325
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10325
    Throughput:     1.61MB/s
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
    Reqs/sec      6133.48    1079.57    8340.84
    Latency        8.15ms     3.92ms   377.71ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.75MB/s
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
    Reqs/sec     20394.88    6186.61   36352.19
    Latency        2.45ms     2.06ms   187.98ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.62MB/s
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
    Reqs/sec     21671.29    6646.75   29387.48
    Latency        2.30ms     2.32ms   202.81ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.90MB/s
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
    Reqs/sec     55900.26    8363.94   66008.21
    Latency        0.89ms   149.04us     3.81ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.94MB/s
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
    Reqs/sec     20593.17   10358.42   78608.27
    Latency        2.42ms     2.49ms   217.95ms
    HTTP codes:
      1xx - 0, 2xx - 89717, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10283
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10283
    Throughput:     4.18MB/s
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
    Reqs/sec     20046.21    4721.52   58979.99
    Latency        2.49ms     1.94ms   168.91ms
    HTTP codes:
      1xx - 0, 2xx - 97362, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2638
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2638
    Throughput:     4.47MB/s
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
    Reqs/sec     70279.30    4411.04   83709.35
    Latency      709.13us   294.41us    15.88ms
    HTTP codes:
      1xx - 0, 2xx - 95795, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4205
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4205
    Throughput:    10.65MB/s
  ```


