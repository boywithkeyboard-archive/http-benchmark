## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73159` | `2536` | `81404` |
| **80%** | [Hyper Express](#hyper-express) | `58250` | `3239` | `61181` |
| **32%** | [Node (Default)](#node-default) | `23348` | `7585` | `66297` |
| **31%** | [Fastify](#fastify) | `22872` | `7424` | `37703` |
| **27%** | [Hono](#hono) | `19864` | `5873` | `29175` |
| **25%** | [Koa](#koa) | `18070` | `7159` | `53069` |
| **11%** | [Carbon](#carbon) | `8042` | `1459` | `10466` |
| **9%** | [Express](#express) | `6450` | `1181` | `10111` |


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
    Reqs/sec      8630.96    5725.25   79263.66
    Latency        5.78ms     4.36ms   377.89ms
    HTTP codes:
      1xx - 0, 2xx - 92359, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7641
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7641
    Throughput:     1.81MB/s
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
    Reqs/sec      6411.10    1168.20    8308.60
    Latency        7.80ms     3.65ms   350.17ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     23889.19    8124.06   38502.22
    Latency        2.09ms     2.13ms   191.21ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.41MB/s
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
    Reqs/sec     21048.77    6371.16   29021.58
    Latency        2.37ms     2.06ms   186.19ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.76MB/s
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
    Reqs/sec     59086.96    3273.47   62163.74
    Latency      844.08us    77.62us     3.34ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.39MB/s
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
    Reqs/sec     18436.90    8583.67   67153.00
    Latency        2.71ms     2.29ms   203.36ms
    HTTP codes:
      1xx - 0, 2xx - 92995, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7005
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7005
    Throughput:     3.88MB/s
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
    Reqs/sec     24642.07    8479.39   82697.31
    Latency        2.03ms     1.96ms   168.53ms
    HTTP codes:
      1xx - 0, 2xx - 96461, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3539
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3539
    Throughput:     5.44MB/s
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
    Reqs/sec     74252.95    2590.38   84821.23
    Latency      670.15us   177.12us    10.52ms
    HTTP codes:
      1xx - 0, 2xx - 96405, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3595
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3595
    Throughput:    11.33MB/s
  ```


