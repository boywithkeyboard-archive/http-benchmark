## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74723` | `4815` | `84572` |
| **83%** | [Hyper Express](#hyper-express) | `61897` | `3166` | `64568` |
| **36%** | [Node (Default)](#node-default) | `26760` | `8678` | `62109` |
| **33%** | [Fastify](#fastify) | `24699` | `7690` | `36502` |
| **28%** | [Hono](#hono) | `21117` | `6170` | `30426` |
| **25%** | [Koa](#koa) | `18671` | `6289` | `50319` |
| **11%** | [Carbon](#carbon) | `8387` | `1470` | `10651` |
| **9%** | [Express](#express) | `6383` | `1004` | `8499` |


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
    Reqs/sec      8847.02    5045.13   58106.11
    Latency        5.64ms     4.34ms   374.60ms
    HTTP codes:
      1xx - 0, 2xx - 92493, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7507
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7507
    Throughput:     1.86MB/s
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
    Reqs/sec      6928.23    4553.17   58176.12
    Latency        7.21ms     3.90ms   353.45ms
    HTTP codes:
      1xx - 0, 2xx - 91810, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8190
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8190
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
    Reqs/sec     24512.71    7364.60   36953.28
    Latency        2.04ms     2.04ms   183.12ms
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
    Reqs/sec     20963.44    5452.88   29572.86
    Latency        2.38ms     2.02ms   178.36ms
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
    Reqs/sec     63009.96    3818.01   70041.35
    Latency      791.29us    69.43us     2.97ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.95MB/s
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
    Reqs/sec     19202.96    7022.63   62403.74
    Latency        2.60ms     2.35ms   203.90ms
    HTTP codes:
      1xx - 0, 2xx - 93645, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6355
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6355
    Throughput:     4.07MB/s
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
    Reqs/sec     25751.59    6949.20   41679.72
    Latency        1.94ms     1.89ms   161.31ms
    HTTP codes:
      1xx - 0, 2xx - 97561, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2439
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2439
    Throughput:     5.74MB/s
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
    Reqs/sec     73616.28    5037.24   81567.21
    Latency      677.09us   167.04us     5.14ms
    HTTP codes:
      1xx - 0, 2xx - 97182, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2818
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2818
    Throughput:    11.31MB/s
  ```


