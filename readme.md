## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73642` | `1831` | `80484` |
| **82%** | [Hyper Express](#hyper-express) | `60256` | `3822` | `64195` |
| **36%** | [Node (Default)](#node-default) | `26288` | `8184` | `65496` |
| **33%** | [Fastify](#fastify) | `24526` | `7666` | `35429` |
| **29%** | [Hono](#hono) | `21078` | `5612` | `29591` |
| **26%** | [Koa](#koa) | `18829` | `8283` | `69380` |
| **11%** | [Carbon](#carbon) | `8413` | `1485` | `10442` |
| **9%** | [Express](#express) | `6410` | `1053` | `8327` |


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
    Reqs/sec      9196.66    7317.29   80231.90
    Latency        5.43ms     4.29ms   369.27ms
    HTTP codes:
      1xx - 0, 2xx - 89130, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10870
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10870
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
    Reqs/sec      7145.50    6244.38   79686.92
    Latency        6.98ms     4.01ms   367.07ms
    HTTP codes:
      1xx - 0, 2xx - 89208, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10792
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10792
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
    Reqs/sec     24768.32    7793.86   35728.53
    Latency        2.02ms     2.00ms   179.75ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.61MB/s
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
    Reqs/sec     21945.66    6160.21   30285.90
    Latency        2.28ms     1.94ms   174.49ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.96MB/s
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
    Reqs/sec     62286.27    3762.25   65054.51
    Latency      798.54us    68.03us     2.94ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.87MB/s
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
    Reqs/sec     20027.10   10399.14   80373.74
    Latency        2.48ms     2.39ms   208.99ms
    HTTP codes:
      1xx - 0, 2xx - 88644, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11356
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11356
    Throughput:     4.03MB/s
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
    Reqs/sec     25778.36    7944.72   74783.95
    Latency        1.94ms     1.85ms   160.22ms
    HTTP codes:
      1xx - 0, 2xx - 96341, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3659
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3659
    Throughput:     5.69MB/s
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
    Reqs/sec     74301.09    3257.14   82255.95
    Latency      670.41us   134.38us     5.65ms
    HTTP codes:
      1xx - 0, 2xx - 97532, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2468
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2468
    Throughput:    11.47MB/s
  ```


