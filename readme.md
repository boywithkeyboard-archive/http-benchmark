## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73733` | `4623` | `81002` |
| **86%** | [Hyper Express](#hyper-express) | `63207` | `5956` | `78973` |
| **35%** | [Node (Default)](#node-default) | `25768` | `6655` | `41823` |
| **34%** | [Fastify](#fastify) | `25028` | `7992` | `36248` |
| **27%** | [Hono](#hono) | `19866` | `5548` | `29545` |
| **25%** | [Koa](#koa) | `18583` | `7170` | `57664` |
| **11%** | [Carbon](#carbon) | `8164` | `1370` | `10407` |
| **9%** | [Express](#express) | `6487` | `1027` | `8409` |


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
    Reqs/sec      8586.76    3852.56   53499.80
    Latency        5.82ms     4.16ms   355.67ms
    HTTP codes:
      1xx - 0, 2xx - 94752, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5248
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5248
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
    Reqs/sec      6470.13    1023.56    8456.56
    Latency        7.72ms     3.54ms   341.46ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     23418.24    7197.91   34979.83
    Latency        2.13ms     1.96ms   178.44ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.31MB/s
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
    Reqs/sec     20507.19    5664.38   28896.17
    Latency        2.44ms     2.02ms   181.12ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.63MB/s
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
    Reqs/sec     64129.56    4358.28   69710.03
    Latency      777.62us    71.01us     3.41ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.11MB/s
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
    Reqs/sec     18936.79    7537.61   63039.88
    Latency        2.63ms     2.41ms   210.18ms
    HTTP codes:
      1xx - 0, 2xx - 92301, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7699
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7699
    Throughput:     3.95MB/s
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
    Reqs/sec     25991.87    7298.61   49002.47
    Latency        1.92ms     1.83ms   157.83ms
    HTTP codes:
      1xx - 0, 2xx - 98017, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1983
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1981
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 2
    Throughput:     5.83MB/s
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
    Reqs/sec     74074.01    6033.74   84070.55
    Latency      671.42us   197.80us    15.01ms
    HTTP codes:
      1xx - 0, 2xx - 95482, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4518
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4518
    Throughput:    11.18MB/s
  ```


