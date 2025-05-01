## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73307` | `4421` | `80444` |
| **83%** | [Hyper Express](#hyper-express) | `61209` | `3293` | `64092` |
| **36%** | [Node (Default)](#node-default) | `26332` | `7933` | `51819` |
| **34%** | [Fastify](#fastify) | `25026` | `7760` | `36381` |
| **29%** | [Hono](#hono) | `21147` | `5501` | `28695` |
| **26%** | [Koa](#koa) | `18874` | `7144` | `56935` |
| **11%** | [Carbon](#carbon) | `8345` | `1402` | `10434` |
| **9%** | [Express](#express) | `6504` | `1022` | `8446` |


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
    Reqs/sec      8877.39    4723.36   55427.23
    Latency        5.62ms     4.20ms   361.93ms
    HTTP codes:
      1xx - 0, 2xx - 92618, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7382
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7382
    Throughput:     1.87MB/s
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
    Reqs/sec      6907.18    3836.01   50902.91
    Latency        7.23ms     3.82ms   346.69ms
    HTTP codes:
      1xx - 0, 2xx - 93392, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6608
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6608
    Throughput:     1.85MB/s
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
    Reqs/sec     23700.82    6530.89   35707.46
    Latency        2.11ms     1.99ms   179.65ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.38MB/s
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
    Reqs/sec     21389.08    5583.66   29123.68
    Latency        2.34ms     1.96ms   175.26ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.83MB/s
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
    Reqs/sec     61301.05    3726.86   63717.21
    Latency      813.26us    79.79us     3.11ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.71MB/s
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
    Reqs/sec     19218.14    7586.22   61932.80
    Latency        2.60ms     2.35ms   206.05ms
    HTTP codes:
      1xx - 0, 2xx - 92489, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7511
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7511
    Throughput:     4.02MB/s
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
    Reqs/sec     26099.85    7569.68   53286.80
    Latency        1.91ms     1.93ms   169.43ms
    HTTP codes:
      1xx - 0, 2xx - 97409, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2591
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2591
    Throughput:     5.82MB/s
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
    Reqs/sec     72481.12    4936.44   81344.65
    Latency      688.06us   163.77us     7.04ms
    HTTP codes:
      1xx - 0, 2xx - 97420, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2580
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2580
    Throughput:    11.16MB/s
  ```


