## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75254` | `6183` | `87889` |
| **85%** | [Hyper Express](#hyper-express) | `63893` | `3413` | `70358` |
| **34%** | [Node (Default)](#node-default) | `25870` | `7407` | `49268` |
| **32%** | [Fastify](#fastify) | `24422` | `7168` | `35788` |
| **29%** | [Hono](#hono) | `21974` | `5811` | `30737` |
| **25%** | [Koa](#koa) | `18439` | `6451` | `51235` |
| **11%** | [Carbon](#carbon) | `8307` | `1382` | `10624` |
| **9%** | [Express](#express) | `6645` | `1061` | `8649` |


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
    Reqs/sec      8959.78    4833.42   59478.44
    Latency        5.56ms     4.15ms   358.47ms
    HTTP codes:
      1xx - 0, 2xx - 92573, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7427
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7427
    Throughput:     1.89MB/s
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
    Reqs/sec      7123.92    3371.17   45706.93
    Latency        7.00ms     3.65ms   334.68ms
    HTTP codes:
      1xx - 0, 2xx - 94589, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5411
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5375
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 36
    Throughput:     1.93MB/s
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
    Reqs/sec     24493.77    7115.31   36109.81
    Latency        2.04ms     1.93ms   173.92ms
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
    Reqs/sec     21118.69    5898.82   29713.24
    Latency        2.37ms     1.94ms   174.92ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.77MB/s
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
    Reqs/sec     63195.59    4339.61   69523.66
    Latency      789.25us   106.90us     6.09ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.97MB/s
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
    Reqs/sec     18083.98    6892.27   56819.15
    Latency        2.76ms     2.27ms   195.88ms
    HTTP codes:
      1xx - 0, 2xx - 92709, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7291
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7291
    Throughput:     3.79MB/s
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
    Reqs/sec     25544.72    7356.83   55105.77
    Latency        1.95ms     1.92ms   164.12ms
    HTTP codes:
      1xx - 0, 2xx - 96887, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3113
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3113
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
    Reqs/sec     74956.64    5915.31   86199.37
    Latency      664.35us   236.67us    14.79ms
    HTTP codes:
      1xx - 0, 2xx - 97480, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2520
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2520
    Throughput:    11.54MB/s
  ```


