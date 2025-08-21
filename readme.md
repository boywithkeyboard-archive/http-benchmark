## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `76019` | `3098` | `83284` |
| **85%** | [Hyper Express](#hyper-express) | `64310` | `3874` | `68441` |
| **35%** | [Node (Default)](#node-default) | `26293` | `8625` | `74753` |
| **32%** | [Fastify](#fastify) | `24135` | `6970` | `35854` |
| **28%** | [Hono](#hono) | `21659` | `5988` | `30790` |
| **26%** | [Koa](#koa) | `19603` | `8529` | `71932` |
| **11%** | [Carbon](#carbon) | `8349` | `1428` | `10508` |
| **9%** | [Express](#express) | `6470` | `1074` | `8464` |


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
    Reqs/sec      8805.00    5407.10   67581.53
    Latency        5.67ms     4.20ms   363.52ms
    HTTP codes:
      1xx - 0, 2xx - 92704, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7296
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7296
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
    Reqs/sec      7146.96    5517.75   67171.64
    Latency        6.99ms     3.90ms   357.26ms
    HTTP codes:
      1xx - 0, 2xx - 91026, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8974
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8974
    Throughput:     1.86MB/s
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
    Reqs/sec     24525.64    7423.14   36412.58
    Latency        2.04ms     1.99ms   179.57ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.57MB/s
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
    Reqs/sec     20629.21    5739.99   29850.84
    Latency        2.42ms     2.01ms   182.39ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.66MB/s
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
    Reqs/sec     62468.82    2956.05   67227.23
    Latency      798.97us    63.90us     2.88ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.87MB/s
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
    Reqs/sec     19871.35    9082.54   71838.45
    Latency        2.51ms     2.40ms   211.53ms
    HTTP codes:
      1xx - 0, 2xx - 91931, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8069
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8069
    Throughput:     4.13MB/s
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
    Reqs/sec     25973.92    7843.32   62418.81
    Latency        1.92ms     1.94ms   164.90ms
    HTTP codes:
      1xx - 0, 2xx - 97511, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2489
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2489
    Throughput:     5.81MB/s
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
    Reqs/sec     76785.44    3502.45   87654.16
    Latency      648.78us   135.06us     6.44ms
    HTTP codes:
      1xx - 0, 2xx - 96976, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3024
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3024
    Throughput:    11.78MB/s
  ```


