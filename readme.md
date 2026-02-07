## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `127664` | `6300` | `141899` |
| **81%** | [Hyper Express](#hyper-express) | `103558` | `7141` | `106646` |
| **31%** | [Node (Default)](#node-default) | `39398` | `10639` | `104647` |
| **29%** | [Fastify](#fastify) | `37452` | `10561` | `57170` |
| **27%** | [Hono](#hono) | `33881` | `7444` | `49211` |
| **26%** | [Koa](#koa) | `33265` | `15229` | `114216` |
| **10%** | [Carbon](#carbon) | `12578` | `2494` | `16955` |
| **7%** | [Express](#express) | `8995` | `1606` | `12059` |


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
    Reqs/sec     13780.04   10385.40  112525.39
    Latency        3.62ms     3.63ms   312.65ms
    HTTP codes:
      1xx - 0, 2xx - 90229, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9771
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9771
    Throughput:     2.82MB/s
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
    Reqs/sec     10654.98   12453.86  122057.01
    Latency        4.68ms     3.22ms   288.94ms
    HTTP codes:
      1xx - 0, 2xx - 83569, 3xx - 0, 4xx - 0, 5xx - 0
      others - 16431
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 16396
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 35
    Throughput:     2.55MB/s
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
    Reqs/sec     37248.25    9416.45   57486.08
    Latency        1.34ms     1.39ms   120.35ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.44MB/s
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
    Reqs/sec     34477.61    7806.96   50652.02
    Latency        1.45ms     1.43ms   124.55ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.78MB/s
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
    Reqs/sec    102012.99    7231.43  105767.58
    Latency      488.27us    44.23us     2.30ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    14.49MB/s
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
    Reqs/sec     32790.28   14173.50  111875.50
    Latency        1.52ms     1.70ms   144.18ms
    HTTP codes:
      1xx - 0, 2xx - 90320, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9680
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9680
    Throughput:     6.71MB/s
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
    Reqs/sec     39996.61   11533.01  107059.90
    Latency        1.25ms     1.37ms   113.73ms
    HTTP codes:
      1xx - 0, 2xx - 95853, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4147
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4147
    Throughput:     8.77MB/s
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
    Reqs/sec    126337.60    7795.89  141173.94
    Latency      392.86us   121.58us     6.49ms
    HTTP codes:
      1xx - 0, 2xx - 96449, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3551
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3551
    Throughput:    19.31MB/s
  ```


