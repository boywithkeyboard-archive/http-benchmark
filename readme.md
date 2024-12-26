## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75136` | `8399` | `93412` |
| **83%** | [Hyper Express](#hyper-express) | `62644` | `3060` | `72943` |
| **33%** | [Node (Default)](#node-default) | `24662` | `6548` | `51107` |
| **33%** | [Fastify](#fastify) | `24518` | `7186` | `34813` |
| **27%** | [Hono](#hono) | `20455` | `5513` | `29675` |
| **24%** | [Koa](#koa) | `17807` | `6027` | `54978` |
| **11%** | [Carbon](#carbon) | `8181` | `1398` | `10399` |
| **9%** | [Express](#express) | `6519` | `1039` | `8470` |


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
    Reqs/sec      8593.36    3322.95   49151.41
    Latency        5.81ms     4.11ms   356.00ms
    HTTP codes:
      1xx - 0, 2xx - 95724, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4276
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4276
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
    Reqs/sec      6504.94    1008.97    8449.28
    Latency        7.69ms     3.49ms   333.84ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     25393.88    7787.10   35464.36
    Latency        1.97ms     2.02ms   183.21ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.75MB/s
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
    Reqs/sec     20840.08    5804.72   29763.42
    Latency        2.40ms     1.87ms   171.31ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.71MB/s
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
    Reqs/sec     62733.62    3947.36   66735.23
    Latency      795.02us    79.72us     4.39ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.91MB/s
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
    Reqs/sec     18733.41    6681.91   51967.57
    Latency        2.66ms     2.23ms   196.23ms
    HTTP codes:
      1xx - 0, 2xx - 94236, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5764
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5764
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
    Reqs/sec     26435.37    7870.38   46978.93
    Latency        1.89ms     1.85ms   157.31ms
    HTTP codes:
      1xx - 0, 2xx - 98140, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1860
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1860
    Throughput:     5.94MB/s
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
    Reqs/sec     72828.23    4759.22   83633.47
    Latency      683.62us   200.03us    10.80ms
    HTTP codes:
      1xx - 0, 2xx - 96120, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3880
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3880
    Throughput:    11.07MB/s
  ```


