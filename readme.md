## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `68770` | `5658` | `84711` |
| **85%** | [Hyper Express](#hyper-express) | `58605` | `3456` | `68225` |
| **31%** | [Hono](#hono) | `21039` | `6824` | `30675` |
| **30%** | [Fastify](#fastify) | `20478` | `5140` | `37898` |
| **29%** | [Node (Default)](#node-default) | `20070` | `4853` | `57177` |
| **26%** | [Koa](#koa) | `17549` | `8190` | `68225` |
| **11%** | [Carbon](#carbon) | `7379` | `1253` | `10448` |
| **9%** | [Express](#express) | `6182` | `1074` | `8249` |


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
    Reqs/sec      8342.82    6664.64   69117.75
    Latency        5.98ms     4.61ms   392.51ms
    HTTP codes:
      1xx - 0, 2xx - 88845, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11155
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11155
    Throughput:     1.68MB/s
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
    Reqs/sec      6109.86    1091.76    8271.55
    Latency        8.18ms     3.75ms   360.06ms
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
    Reqs/sec     20359.34    5365.75   36793.00
    Latency        2.46ms     2.05ms   184.90ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.61MB/s
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
    Reqs/sec     21990.62    6472.73   30686.91
    Latency        2.27ms     2.42ms   209.99ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.97MB/s
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
    Reqs/sec     58323.53    3348.11   70608.42
    Latency        0.85ms    95.23us     3.85ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.29MB/s
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
    Reqs/sec     19548.25    9977.33   77925.99
    Latency        2.55ms     2.59ms   224.27ms
    HTTP codes:
      1xx - 0, 2xx - 89962, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10038
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10038
    Throughput:     3.98MB/s
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
    Reqs/sec     20141.41    5417.57   66117.97
    Latency        2.47ms     2.05ms   182.74ms
    HTTP codes:
      1xx - 0, 2xx - 96251, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3749
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3749
    Throughput:     4.44MB/s
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
    Reqs/sec     69782.60    4542.11   85602.68
    Latency      713.71us   198.00us    10.78ms
    HTTP codes:
      1xx - 0, 2xx - 96676, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3324
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3324
    Throughput:    10.67MB/s
  ```


