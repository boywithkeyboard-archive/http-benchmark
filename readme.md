## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75018` | `5737` | `85157` |
| **85%** | [Hyper Express](#hyper-express) | `63484` | `5073` | `86555` |
| **35%** | [Node (Default)](#node-default) | `26456` | `8021` | `49415` |
| **31%** | [Fastify](#fastify) | `23027` | `6998` | `34433` |
| **28%** | [Hono](#hono) | `20686` | `5926` | `29364` |
| **24%** | [Koa](#koa) | `18003` | `6671` | `53019` |
| **11%** | [Carbon](#carbon) | `8124` | `1387` | `10218` |
| **8%** | [Express](#express) | `6315` | `991` | `8347` |


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
    Reqs/sec      8588.06    4030.57   47926.64
    Latency        5.81ms     4.18ms   360.05ms
    HTTP codes:
      1xx - 0, 2xx - 94273, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5727
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5727
    Throughput:     1.84MB/s
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
    Reqs/sec      6259.58     946.23    8137.90
    Latency        7.99ms     3.54ms   341.82ms
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
    Reqs/sec     23366.14    7179.75   35297.07
    Latency        2.14ms     1.92ms   176.45ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.30MB/s
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
    Reqs/sec     20250.10    5437.71   28845.61
    Latency        2.47ms     2.03ms   182.05ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.57MB/s
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
    Reqs/sec     63571.36    3964.40   77001.91
    Latency      784.29us    70.37us     2.67ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.03MB/s
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
    Reqs/sec     17097.33    5742.39   43346.13
    Latency        2.92ms     2.36ms   209.61ms
    HTTP codes:
      1xx - 0, 2xx - 95064, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4936
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4936
    Throughput:     3.67MB/s
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
    Reqs/sec     25257.91    7677.48   56192.45
    Latency        1.98ms     1.85ms   158.76ms
    HTTP codes:
      1xx - 0, 2xx - 97463, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2537
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2528
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 9
    Throughput:     5.64MB/s
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
    Reqs/sec     74755.82    7208.74   85735.38
    Latency      670.15us   547.39us    33.24ms
    HTTP codes:
      1xx - 0, 2xx - 96891, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3109
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3109
    Throughput:    11.39MB/s
  ```


