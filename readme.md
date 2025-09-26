## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `79765` | `5280` | `96841` |
| **80%** | [Hyper Express](#hyper-express) | `63441` | `3460` | `66461` |
| **33%** | [Node (Default)](#node-default) | `26601` | `9295` | `78983` |
| **30%** | [Fastify](#fastify) | `23742` | `7032` | `35240` |
| **27%** | [Hono](#hono) | `21316` | `5408` | `30871` |
| **24%** | [Koa](#koa) | `19028` | `9088` | `77937` |
| **11%** | [Carbon](#carbon) | `8508` | `1495` | `10572` |
| **8%** | [Express](#express) | `6479` | `1041` | `8444` |


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
    Reqs/sec      9223.23    6203.45   76299.74
    Latency        5.41ms     4.33ms   374.13ms
    HTTP codes:
      1xx - 0, 2xx - 91449, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8551
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8551
    Throughput:     1.92MB/s
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
    Reqs/sec      7214.98    6095.30   78521.00
    Latency        6.91ms     3.89ms   353.09ms
    HTTP codes:
      1xx - 0, 2xx - 89611, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10389
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10389
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
    Reqs/sec     24162.37    7135.81   36263.56
    Latency        2.07ms     1.94ms   175.38ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.48MB/s
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
    Reqs/sec     21049.20    6127.73   29930.12
    Latency        2.37ms     1.91ms   174.44ms
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
    Reqs/sec     64242.42    3781.58   67954.90
    Latency      776.16us    70.19us     3.46ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.12MB/s
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
    Reqs/sec     19086.94    9171.68   79983.22
    Latency        2.61ms     2.34ms   203.81ms
    HTTP codes:
      1xx - 0, 2xx - 91432, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8568
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8568
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
    Reqs/sec     25979.75    8400.53   77860.22
    Latency        1.92ms     1.95ms   167.68ms
    HTTP codes:
      1xx - 0, 2xx - 96345, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3655
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3655
    Throughput:     5.73MB/s
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
    Reqs/sec     78174.79    4346.00   91118.48
    Latency      637.34us   149.29us     7.16ms
    HTTP codes:
      1xx - 0, 2xx - 97416, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2584
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2584
    Throughput:    12.05MB/s
  ```


