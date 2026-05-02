## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `70156` | `3846` | `78907` |
| **84%** | [Hyper Express](#hyper-express) | `58846` | `4148` | `66752` |
| **30%** | [Hono](#hono) | `21383` | `6399` | `30816` |
| **28%** | [Fastify](#fastify) | `19804` | `5223` | `36976` |
| **28%** | [Node (Default)](#node-default) | `19645` | `5011` | `66409` |
| **26%** | [Koa](#koa) | `18537` | `8251` | `64603` |
| **11%** | [Carbon](#carbon) | `7441` | `1221` | `10271` |
| **9%** | [Express](#express) | `6131` | `1046` | `8194` |


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
    Reqs/sec      7671.98    5284.21   69221.89
    Latency        6.50ms     4.44ms   380.37ms
    HTTP codes:
      1xx - 0, 2xx - 92229, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7771
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7771
    Throughput:     1.61MB/s
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
    Reqs/sec      6097.99    1022.15    8191.75
    Latency        8.19ms     3.77ms   363.62ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.74MB/s
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
    Reqs/sec     19887.34    4840.19   36839.81
    Latency        2.51ms     1.97ms   179.88ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.52MB/s
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
    Reqs/sec     21781.38    6774.20   31460.25
    Latency        2.29ms     2.14ms   190.63ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.92MB/s
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
    Reqs/sec     59302.44    3670.86   66637.52
    Latency      840.93us    93.05us     3.76ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.42MB/s
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
    Reqs/sec     17496.72    7888.34   64881.85
    Latency        2.85ms     2.44ms   214.80ms
    HTTP codes:
      1xx - 0, 2xx - 93380, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6620
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6620
    Throughput:     3.70MB/s
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
    Reqs/sec     20660.15    5695.38   68613.92
    Latency        2.41ms     2.13ms   182.67ms
    HTTP codes:
      1xx - 0, 2xx - 96507, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3493
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3493
    Throughput:     4.57MB/s
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
    Reqs/sec     71141.67    4838.43   85562.31
    Latency      700.37us   164.95us     8.75ms
    HTTP codes:
      1xx - 0, 2xx - 96941, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3059
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3059
    Throughput:    10.91MB/s
  ```


