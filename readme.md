## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73541` | `3199` | `80974` |
| **83%** | [Hyper Express](#hyper-express) | `61151` | `3494` | `65922` |
| **33%** | [Node (Default)](#node-default) | `24289` | `7525` | `62732` |
| **32%** | [Fastify](#fastify) | `23386` | `7080` | `34977` |
| **27%** | [Hono](#hono) | `19535` | `5259` | `27966` |
| **25%** | [Koa](#koa) | `18328` | `9093` | `79610` |
| **11%** | [Carbon](#carbon) | `7966` | `1397` | `10279` |
| **8%** | [Express](#express) | `6196` | `1004` | `8082` |


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
    Reqs/sec      8539.12    5897.13   74834.36
    Latency        5.84ms     4.42ms   382.16ms
    HTTP codes:
      1xx - 0, 2xx - 91750, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8250
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8250
    Throughput:     1.78MB/s
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
    Reqs/sec      6124.41     949.05    8143.42
    Latency        8.16ms     3.69ms   356.39ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.75MB/s
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
    Reqs/sec     23046.45    7025.25   34621.05
    Latency        2.17ms     2.06ms   186.19ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.23MB/s
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
    Reqs/sec     19643.93    5144.53   28395.24
    Latency        2.54ms     2.09ms   187.97ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.44MB/s
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
    Reqs/sec     61707.32    3646.39   64957.17
    Latency      808.12us    77.51us     4.47ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.76MB/s
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
    Reqs/sec     18381.11    7685.14   64544.08
    Latency        2.71ms     2.46ms   216.00ms
    HTTP codes:
      1xx - 0, 2xx - 93142, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6858
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6858
    Throughput:     3.87MB/s
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
    Reqs/sec     24300.47    7303.92   62911.01
    Latency        2.05ms     1.95ms   172.00ms
    HTTP codes:
      1xx - 0, 2xx - 97366, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2634
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2634
    Throughput:     5.43MB/s
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
    Reqs/sec     73761.39    3276.56   84132.94
    Latency      675.57us   137.20us     4.75ms
    HTTP codes:
      1xx - 0, 2xx - 97324, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2676
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2676
    Throughput:    11.36MB/s
  ```


