## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73829` | `5217` | `86889` |
| **83%** | [Hyper Express](#hyper-express) | `61305` | `4410` | `68080` |
| **34%** | [Node (Default)](#node-default) | `25055` | `7073` | `47900` |
| **33%** | [Fastify](#fastify) | `24352` | `7208` | `35788` |
| **28%** | [Hono](#hono) | `20676` | `5323` | `29145` |
| **25%** | [Koa](#koa) | `18682` | `6396` | `50220` |
| **11%** | [Carbon](#carbon) | `8144` | `1384` | `10219` |
| **9%** | [Express](#express) | `6297` | `1019` | `8284` |


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
    Reqs/sec      8573.71    4503.80   51191.41
    Latency        5.82ms     4.45ms   383.72ms
    HTTP codes:
      1xx - 0, 2xx - 93375, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6625
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6625
    Throughput:     1.82MB/s
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
    Reqs/sec      6257.73     979.63    8236.26
    Latency        7.99ms     3.85ms   365.63ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.79MB/s
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
    Reqs/sec     25141.67    7949.91   35335.94
    Latency        1.99ms     2.20ms   193.50ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.70MB/s
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
    Reqs/sec     21213.70    6007.59   29224.80
    Latency        2.36ms     2.04ms   185.27ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.79MB/s
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
    Reqs/sec     62945.29    3623.78   69104.12
    Latency      792.12us    70.72us     3.17ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.94MB/s
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
    Reqs/sec     18285.34    6476.14   50824.26
    Latency        2.73ms     2.41ms   212.19ms
    HTTP codes:
      1xx - 0, 2xx - 94294, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5706
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5706
    Throughput:     3.90MB/s
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
    Reqs/sec     25693.29    7475.48   55384.58
    Latency        1.94ms     1.89ms   160.06ms
    HTTP codes:
      1xx - 0, 2xx - 96203, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3797
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3797
    Throughput:     5.66MB/s
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
    Reqs/sec     74593.80    5423.33   84143.54
    Latency      668.71us   214.67us    10.75ms
    HTTP codes:
      1xx - 0, 2xx - 97860, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2140
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2140
    Throughput:    11.53MB/s
  ```


