## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74581` | `5815` | `87547` |
| **86%** | [Hyper Express](#hyper-express) | `64471` | `3924` | `72463` |
| **36%** | [Node (Default)](#node-default) | `26757` | `7132` | `50723` |
| **33%** | [Fastify](#fastify) | `24566` | `7583` | `36967` |
| **28%** | [Hono](#hono) | `20948` | `5497` | `30100` |
| **26%** | [Koa](#koa) | `19161` | `7052` | `57585` |
| **11%** | [Carbon](#carbon) | `8459` | `1453` | `10456` |
| **9%** | [Express](#express) | `6387` | `995` | `8386` |


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
    Reqs/sec      8725.23    4321.75   51145.84
    Latency        5.72ms     4.23ms   365.12ms
    HTTP codes:
      1xx - 0, 2xx - 93621, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6379
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6379
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
    Reqs/sec      6969.24    4567.78   57430.84
    Latency        7.16ms     4.04ms   365.15ms
    HTTP codes:
      1xx - 0, 2xx - 91794, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8206
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8206
    Throughput:     1.83MB/s
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
    Reqs/sec     23958.40    7134.98   36805.22
    Latency        2.09ms     1.99ms   181.13ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.43MB/s
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
    Reqs/sec     21480.62    6008.19   30683.21
    Latency        2.33ms     2.00ms   179.21ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.85MB/s
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
    Reqs/sec     63666.30    4028.06   69733.01
    Latency      782.98us    66.45us     3.88ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.05MB/s
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
    Reqs/sec     19685.12    6667.02   54877.52
    Latency        2.53ms     2.23ms   195.40ms
    HTTP codes:
      1xx - 0, 2xx - 94338, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5662
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5662
    Throughput:     4.20MB/s
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
    Reqs/sec     26624.24    7774.23   47940.24
    Latency        1.87ms     1.90ms   157.89ms
    HTTP codes:
      1xx - 0, 2xx - 97582, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2418
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2418
    Throughput:     5.95MB/s
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
    Reqs/sec     74747.75    4691.32   84684.71
    Latency      666.11us   171.59us     8.12ms
    HTTP codes:
      1xx - 0, 2xx - 96776, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3224
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3224
    Throughput:    11.44MB/s
  ```


