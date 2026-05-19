## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `81401` | `2668` | `85549` |
| **85%** | [Hyper Express](#hyper-express) | `69560` | `3948` | `82522` |
| **45%** | [Node (Default)](#node-default) | `36341` | `11780` | `77851` |
| **39%** | [Fastify](#fastify) | `31646` | `9035` | `52162` |
| **37%** | [Hono](#hono) | `29978` | `9400` | `47018` |
| **36%** | [Koa](#koa) | `29397` | `12157` | `74984` |
| **11%** | [Carbon](#carbon) | `9333` | `2432` | `13430` |
| **9%** | [Express](#express) | `7261` | `1720` | `10605` |


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
    Reqs/sec     10143.49    6797.52   82257.40
    Latency        4.92ms     4.44ms   380.90ms
    HTTP codes:
      1xx - 0, 2xx - 92002, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7998
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7998
    Throughput:     2.12MB/s
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
    Reqs/sec      8032.99    6601.22   77681.13
    Latency        6.21ms     4.07ms   369.79ms
    HTTP codes:
      1xx - 0, 2xx - 89948, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10052
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10052
    Throughput:     2.07MB/s
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
    Reqs/sec     31660.43   10685.78   51643.30
    Latency        1.58ms     1.92ms   169.43ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.18MB/s
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
    Reqs/sec     29668.75    9740.37   45795.56
    Latency        1.69ms     2.16ms   189.52ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     6.70MB/s
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
    Reqs/sec     69430.84    3127.71   71874.61
    Latency      718.34us    67.14us     2.71ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.86MB/s
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
    Reqs/sec     30217.26   12681.11   78122.84
    Latency        1.65ms     2.36ms   205.97ms
    HTTP codes:
      1xx - 0, 2xx - 91320, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8680
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8680
    Throughput:     6.23MB/s
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
    Reqs/sec     34704.64    9526.79   76591.30
    Latency        1.44ms     1.81ms   152.50ms
    HTTP codes:
      1xx - 0, 2xx - 96298, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3702
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3702
    Throughput:     7.65MB/s
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
    Reqs/sec     80792.27    2675.24   85985.75
    Latency      616.83us   125.84us     5.86ms
    HTTP codes:
      1xx - 0, 2xx - 97029, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2971
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2971
    Throughput:    12.41MB/s
  ```


