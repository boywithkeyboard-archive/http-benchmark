## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73301` | `2531` | `78012` |
| **83%** | [Hyper Express](#hyper-express) | `60880` | `3391` | `65330` |
| **36%** | [Node (Default)](#node-default) | `26517` | `9306` | `79902` |
| **32%** | [Fastify](#fastify) | `23177` | `7040` | `35300` |
| **29%** | [Hono](#hono) | `21158` | `6146` | `31040` |
| **26%** | [Koa](#koa) | `18909` | `8137` | `66615` |
| **12%** | [Carbon](#carbon) | `8448` | `1477` | `10465` |
| **9%** | [Express](#express) | `6380` | `1012` | `8482` |


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
    Reqs/sec      9080.46    6108.41   69783.84
    Latency        5.50ms     4.35ms   374.93ms
    HTTP codes:
      1xx - 0, 2xx - 91381, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8619
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8619
    Throughput:     1.88MB/s
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
    Reqs/sec      6853.00    5244.60   67280.41
    Latency        7.28ms     3.90ms   356.10ms
    HTTP codes:
      1xx - 0, 2xx - 91350, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8650
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8650
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
    Reqs/sec     24619.12    7246.56   36684.68
    Latency        2.03ms     1.95ms   179.06ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.59MB/s
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
    Reqs/sec     20812.70    5830.33   29872.55
    Latency        2.40ms     2.08ms   185.90ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.70MB/s
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
    Reqs/sec     61237.89    3477.58   64668.25
    Latency      814.50us    73.27us     2.99ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.70MB/s
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
    Reqs/sec     19217.25    7945.36   68224.93
    Latency        2.60ms     2.29ms   202.68ms
    HTTP codes:
      1xx - 0, 2xx - 93187, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6813
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6813
    Throughput:     4.05MB/s
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
    Reqs/sec     25479.55    7695.65   65616.05
    Latency        1.96ms     2.00ms   171.05ms
    HTTP codes:
      1xx - 0, 2xx - 97056, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2944
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2944
    Throughput:     5.65MB/s
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
    Reqs/sec     74451.05    2750.90   85365.80
    Latency      668.84us   109.51us     4.45ms
    HTTP codes:
      1xx - 0, 2xx - 97096, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2904
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2904
    Throughput:    11.44MB/s
  ```


