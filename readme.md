## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74574` | `2675` | `80257` |
| **83%** | [Hyper Express](#hyper-express) | `61960` | `3730` | `65893` |
| **35%** | [Node (Default)](#node-default) | `25765` | `8443` | `78383` |
| **33%** | [Fastify](#fastify) | `24238` | `7458` | `35279` |
| **29%** | [Hono](#hono) | `21473` | `6257` | `31379` |
| **24%** | [Koa](#koa) | `17996` | `7217` | `63353` |
| **11%** | [Carbon](#carbon) | `8178` | `1459` | `10469` |
| **8%** | [Express](#express) | `6328` | `1073` | `8366` |


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
    Reqs/sec      8869.02    6322.22   75623.60
    Latency        5.62ms     4.37ms   381.83ms
    HTTP codes:
      1xx - 0, 2xx - 91051, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8949
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8949
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
    Reqs/sec      6272.51    1012.81    8341.82
    Latency        7.97ms     3.70ms   356.58ms
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
    Reqs/sec     24573.00    7699.98   35602.68
    Latency        2.03ms     2.06ms   185.62ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.58MB/s
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
    Reqs/sec     21139.87    6430.70   31178.97
    Latency        2.36ms     2.18ms   196.18ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.78MB/s
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
    Reqs/sec     62593.56    3701.75   69342.74
    Latency      796.61us    73.12us     3.41ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.89MB/s
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
    Reqs/sec     19180.40    8917.59   76476.79
    Latency        2.60ms     2.39ms   210.62ms
    HTTP codes:
      1xx - 0, 2xx - 91925, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8075
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8075
    Throughput:     3.99MB/s
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
    Reqs/sec     26787.33   10438.11   82892.41
    Latency        1.86ms     1.92ms   163.86ms
    HTTP codes:
      1xx - 0, 2xx - 93794, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6206
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6206
    Throughput:     5.76MB/s
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
    Reqs/sec     75377.50    2668.86   82266.42
    Latency      660.35us   192.70us     9.20ms
    HTTP codes:
      1xx - 0, 2xx - 96577, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3423
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3423
    Throughput:    11.53MB/s
  ```


