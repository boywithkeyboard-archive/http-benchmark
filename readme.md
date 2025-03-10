## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74498` | `5252` | `83541` |
| **86%** | [Hyper Express](#hyper-express) | `64282` | `5389` | `84739` |
| **36%** | [Node (Default)](#node-default) | `26948` | `7858` | `57087` |
| **33%** | [Fastify](#fastify) | `24299` | `7126` | `36846` |
| **28%** | [Hono](#hono) | `20842` | `5420` | `31148` |
| **25%** | [Koa](#koa) | `18432` | `6363` | `55219` |
| **11%** | [Carbon](#carbon) | `8446` | `1460` | `10557` |
| **9%** | [Express](#express) | `6373` | `1019` | `8458` |


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
    Reqs/sec      8650.18    4302.78   56910.63
    Latency        5.76ms     4.34ms   376.51ms
    HTTP codes:
      1xx - 0, 2xx - 94112, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5888
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5888
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
    Reqs/sec      6435.04    1052.12    8478.41
    Latency        7.77ms     3.56ms   341.75ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     23530.35    6478.83   35879.06
    Latency        2.12ms     2.04ms   184.29ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.34MB/s
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
    Reqs/sec     19623.68    5025.02   29831.91
    Latency        2.55ms     2.15ms   191.93ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.43MB/s
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
    Reqs/sec     63745.89    3899.66   66284.36
    Latency      781.99us    70.79us     3.20ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.06MB/s
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
    Reqs/sec     19475.90    7027.44   55992.95
    Latency        2.56ms     2.37ms   208.51ms
    HTTP codes:
      1xx - 0, 2xx - 93394, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6606
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6606
    Throughput:     4.12MB/s
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
    Reqs/sec     25942.22    7180.53   45167.95
    Latency        1.92ms     1.84ms   157.96ms
    HTTP codes:
      1xx - 0, 2xx - 98147, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1853
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1853
    Throughput:     5.83MB/s
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
    Reqs/sec     72979.78    5496.81   89195.94
    Latency      682.83us   211.60us     9.43ms
    HTTP codes:
      1xx - 0, 2xx - 96707, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3293
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3293
    Throughput:    11.16MB/s
  ```


