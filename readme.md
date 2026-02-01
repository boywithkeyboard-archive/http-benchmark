## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72120` | `2372` | `76931` |
| **81%** | [Hyper Express](#hyper-express) | `58187` | `2505` | `60247` |
| **33%** | [Node (Default)](#node-default) | `23848` | `7109` | `75074` |
| **32%** | [Fastify](#fastify) | `23163` | `7632` | `36495` |
| **28%** | [Hono](#hono) | `20517` | `6060` | `30861` |
| **25%** | [Koa](#koa) | `17826` | `8456` | `78905` |
| **11%** | [Carbon](#carbon) | `8058` | `1500` | `10483` |
| **9%** | [Express](#express) | `6488` | `1196` | `8527` |


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
    Reqs/sec      8741.89    5820.35   73004.42
    Latency        5.71ms     4.34ms   373.22ms
    HTTP codes:
      1xx - 0, 2xx - 92063, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7937
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7937
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
    Reqs/sec      6299.86    1087.58    8556.52
    Latency        7.94ms     3.80ms   361.84ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     23422.49    7569.81   35796.28
    Latency        2.13ms     2.09ms   188.10ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.32MB/s
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
    Reqs/sec     21292.79    6765.04   30963.00
    Latency        2.34ms     2.08ms   189.62ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.81MB/s
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
    Reqs/sec     57529.78    2854.09   59660.13
    Latency        0.87ms    72.88us     2.88ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.17MB/s
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
    Reqs/sec     18626.57    9143.70   80777.54
    Latency        2.68ms     2.45ms   215.57ms
    HTTP codes:
      1xx - 0, 2xx - 91650, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8350
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8350
    Throughput:     3.86MB/s
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
    Reqs/sec     24013.41    7698.99   78159.63
    Latency        2.08ms     1.95ms   164.73ms
    HTTP codes:
      1xx - 0, 2xx - 96270, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3730
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3730
    Throughput:     5.29MB/s
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
    Reqs/sec     72237.59    2953.91   82495.41
    Latency      687.43us   174.02us    10.22ms
    HTTP codes:
      1xx - 0, 2xx - 96161, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3839
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3839
    Throughput:    11.01MB/s
  ```


