## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73356` | `5214` | `83452` |
| **84%** | [Hyper Express](#hyper-express) | `61329` | `3656` | `64782` |
| **35%** | [Node (Default)](#node-default) | `25570` | `7292` | `46124` |
| **34%** | [Fastify](#fastify) | `24690` | `7849` | `37379` |
| **29%** | [Hono](#hono) | `21301` | `6425` | `31430` |
| **25%** | [Koa](#koa) | `18513` | `6509` | `50645` |
| **11%** | [Carbon](#carbon) | `7984` | `1369` | `10296` |
| **8%** | [Express](#express) | `6201` | `966` | `8335` |


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
    Reqs/sec      8434.51    4551.71   52566.93
    Latency        5.91ms     4.36ms   377.57ms
    HTTP codes:
      1xx - 0, 2xx - 92784, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7216
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7216
    Throughput:     1.78MB/s
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
    Reqs/sec      6370.74    1092.51    8396.31
    Latency        7.84ms     3.80ms   363.02ms
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
    Reqs/sec     24535.81    7826.41   36585.27
    Latency        2.04ms     2.14ms   191.46ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.56MB/s
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
    Reqs/sec     20535.66    5818.16   29643.50
    Latency        2.43ms     1.95ms   178.03ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.64MB/s
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
    Reqs/sec     61586.35    3789.55   65503.61
    Latency      808.69us    72.97us     3.16ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.76MB/s
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
    Reqs/sec     19414.07    7100.86   51875.13
    Latency        2.57ms     2.45ms   213.88ms
    HTTP codes:
      1xx - 0, 2xx - 94170, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5830
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5830
    Throughput:     4.14MB/s
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
    Reqs/sec     26624.20    7626.82   50208.43
    Latency        1.87ms     1.92ms   168.57ms
    HTTP codes:
      1xx - 0, 2xx - 97818, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2182
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2182
    Throughput:     5.97MB/s
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
    Reqs/sec     74053.36    4855.30   81268.26
    Latency      672.93us   171.57us     7.09ms
    HTTP codes:
      1xx - 0, 2xx - 97512, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2488
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2488
    Throughput:    11.42MB/s
  ```


