## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `68555` | `5319` | `83614` |
| **85%** | [Hyper Express](#hyper-express) | `58364` | `3374` | `67160` |
| **30%** | [Hono](#hono) | `20806` | `6132` | `29725` |
| **30%** | [Node (Default)](#node-default) | `20672` | `5802` | `65288` |
| **30%** | [Fastify](#fastify) | `20451` | `5435` | `35941` |
| **25%** | [Koa](#koa) | `17270` | `7425` | `61902` |
| **11%** | [Carbon](#carbon) | `7392` | `1350` | `10228` |
| **9%** | [Express](#express) | `6300` | `1148` | `8238` |


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
    Reqs/sec      7533.75    4849.75   59570.86
    Latency        6.63ms     4.86ms   414.31ms
    HTTP codes:
      1xx - 0, 2xx - 92837, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7163
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7163
    Throughput:     1.59MB/s
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
    Reqs/sec      6053.06    1031.66    8263.74
    Latency        8.26ms     3.82ms   367.70ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.73MB/s
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
    Reqs/sec     20279.05    5033.20   35954.22
    Latency        2.46ms     2.24ms   200.97ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.60MB/s
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
    Reqs/sec     19941.53    5714.95   30253.23
    Latency        2.50ms     2.24ms   200.68ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.51MB/s
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
    Reqs/sec     57660.11    3384.23   69522.31
    Latency        0.86ms    96.04us     4.16ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.19MB/s
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
    Reqs/sec     18616.34    8982.36   81845.21
    Latency        2.68ms     2.62ms   227.00ms
    HTTP codes:
      1xx - 0, 2xx - 91104, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8896
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8896
    Throughput:     3.84MB/s
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
    Reqs/sec     20646.64    5154.92   59919.98
    Latency        2.42ms     2.05ms   175.24ms
    HTTP codes:
      1xx - 0, 2xx - 97331, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2669
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2669
    Throughput:     4.60MB/s
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
    Reqs/sec     69589.02    4707.15   85787.80
    Latency      714.19us   213.45us    13.66ms
    HTTP codes:
      1xx - 0, 2xx - 96084, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3916
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3916
    Throughput:    10.59MB/s
  ```


