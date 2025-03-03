## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74369` | `5214` | `83470` |
| **84%** | [Hyper Express](#hyper-express) | `62622` | `3469` | `68223` |
| **35%** | [Node (Default)](#node-default) | `25860` | `7069` | `50316` |
| **33%** | [Fastify](#fastify) | `24216` | `7034` | `36806` |
| **29%** | [Hono](#hono) | `21259` | `5593` | `30068` |
| **25%** | [Koa](#koa) | `18494` | `7746` | `64431` |
| **11%** | [Carbon](#carbon) | `8351` | `1419` | `10406` |
| **9%** | [Express](#express) | `6561` | `1072` | `8547` |


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
    Reqs/sec      8760.17    4665.42   58031.12
    Latency        5.70ms     4.22ms   366.78ms
    HTTP codes:
      1xx - 0, 2xx - 93739, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6261
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6261
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
    Reqs/sec      6497.76    1056.59    8491.30
    Latency        7.69ms     3.65ms   347.18ms
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
    Reqs/sec     24536.25    7413.86   36670.08
    Latency        2.04ms     2.04ms   183.16ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.57MB/s
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
    Reqs/sec     20527.89    5412.61   28826.39
    Latency        2.43ms     2.01ms   178.17ms
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
    Reqs/sec     63344.82    3775.60   68634.96
    Latency      787.38us    72.35us     4.50ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.00MB/s
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
    Reqs/sec     19679.74    7504.02   59901.74
    Latency        2.53ms     2.39ms   206.30ms
    HTTP codes:
      1xx - 0, 2xx - 92343, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7657
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7657
    Throughput:     4.12MB/s
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
    Reqs/sec     27644.14    7913.22   52919.89
    Latency        1.80ms     1.82ms   155.51ms
    HTTP codes:
      1xx - 0, 2xx - 97461, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2539
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2539
    Throughput:     6.17MB/s
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
    Reqs/sec     74592.31    5304.80   84380.84
    Latency      668.81us   194.92us     8.14ms
    HTTP codes:
      1xx - 0, 2xx - 96909, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3091
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3091
    Throughput:    11.41MB/s
  ```


