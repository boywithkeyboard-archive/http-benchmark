## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72867` | `4772` | `82056` |
| **85%** | [Hyper Express](#hyper-express) | `61748` | `3475` | `66692` |
| **34%** | [Node (Default)](#node-default) | `24823` | `6970` | `50795` |
| **33%** | [Fastify](#fastify) | `24148` | `6727` | `36190` |
| **26%** | [Hono](#hono) | `19125` | `5029` | `29292` |
| **25%** | [Koa](#koa) | `18504` | `6682` | `52176` |
| **11%** | [Carbon](#carbon) | `8122` | `1420` | `10170` |
| **9%** | [Express](#express) | `6447` | `1072` | `8372` |


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
    Reqs/sec      8809.86    5584.31   64134.13
    Latency        5.67ms     4.39ms   377.98ms
    HTTP codes:
      1xx - 0, 2xx - 90354, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9646
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9646
    Throughput:     1.81MB/s
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
    Reqs/sec      6347.51    1016.35    8359.23
    Latency        7.87ms     3.69ms   352.13ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.82MB/s
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
    Reqs/sec     23736.75    7010.83   36093.76
    Latency        2.10ms     1.95ms   176.33ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.39MB/s
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
    Reqs/sec     20982.49    6338.11   30242.79
    Latency        2.38ms     2.11ms   188.26ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.74MB/s
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
    Reqs/sec     61580.45    3596.01   65002.21
    Latency      810.24us    77.29us     3.10ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.74MB/s
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
    Reqs/sec     17924.84    6133.82   53848.17
    Latency        2.78ms     2.39ms   208.78ms
    HTTP codes:
      1xx - 0, 2xx - 94426, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5574
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5574
    Throughput:     3.83MB/s
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
    Reqs/sec     23781.71    5994.61   50943.94
    Latency        2.10ms     1.92ms   166.75ms
    HTTP codes:
      1xx - 0, 2xx - 97807, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2193
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2193
    Throughput:     5.33MB/s
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
    Reqs/sec     72558.60    4514.93   77985.84
    Latency      686.32us   179.89us     6.41ms
    HTTP codes:
      1xx - 0, 2xx - 96760, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3240
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3240
    Throughput:    11.11MB/s
  ```


