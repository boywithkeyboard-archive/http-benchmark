## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74564` | `5473` | `86769` |
| **83%** | [Hyper Express](#hyper-express) | `62220` | `2926` | `65003` |
| **35%** | [Node (Default)](#node-default) | `26368` | `8580` | `47323` |
| **33%** | [Fastify](#fastify) | `24309` | `7400` | `35093` |
| **27%** | [Hono](#hono) | `19958` | `5494` | `28964` |
| **26%** | [Koa](#koa) | `19253` | `8067` | `59910` |
| **11%** | [Carbon](#carbon) | `8053` | `1365` | `10192` |
| **9%** | [Express](#express) | `6435` | `1019` | `9493` |


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
    Reqs/sec      8680.73    4504.58   59188.47
    Latency        5.75ms     4.29ms   368.93ms
    HTTP codes:
      1xx - 0, 2xx - 93583, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6417
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6417
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
    Reqs/sec      7090.73    4652.13   55963.77
    Latency        7.04ms     3.85ms   352.48ms
    HTTP codes:
      1xx - 0, 2xx - 91559, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8441
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8441
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
    Reqs/sec     24006.75    7241.80   36203.17
    Latency        2.08ms     1.99ms   180.97ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.45MB/s
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
    Reqs/sec     20387.34    5158.89   29527.92
    Latency        2.45ms     2.03ms   182.64ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.61MB/s
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
    Reqs/sec     62049.18    2639.78   65490.02
    Latency      803.18us    67.70us     4.13ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.82MB/s
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
    Reqs/sec     18653.10    6731.53   55213.08
    Latency        2.67ms     2.37ms   207.35ms
    HTTP codes:
      1xx - 0, 2xx - 92811, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7189
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7189
    Throughput:     3.92MB/s
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
    Reqs/sec     27385.04    8830.57   59170.28
    Latency        1.82ms     1.91ms   161.30ms
    HTTP codes:
      1xx - 0, 2xx - 96434, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3566
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3566
    Throughput:     6.05MB/s
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
    Reqs/sec     75185.73    5899.45   87723.38
    Latency      661.85us   186.39us     9.64ms
    HTTP codes:
      1xx - 0, 2xx - 95993, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4007
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4007
    Throughput:    11.43MB/s
  ```


