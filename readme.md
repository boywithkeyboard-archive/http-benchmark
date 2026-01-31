## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75091` | `2770` | `81680` |
| **84%** | [Hyper Express](#hyper-express) | `63081` | `3884` | `69127` |
| **35%** | [Node (Default)](#node-default) | `26522` | `9169` | `81388` |
| **33%** | [Fastify](#fastify) | `24596` | `7630` | `37138` |
| **27%** | [Hono](#hono) | `20200` | `5084` | `29197` |
| **26%** | [Koa](#koa) | `19858` | `8772` | `68199` |
| **11%** | [Carbon](#carbon) | `8083` | `1392` | `10312` |
| **9%** | [Express](#express) | `6636` | `1126` | `8450` |


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
    Reqs/sec      9027.45    5643.70   64993.79
    Latency        5.53ms     4.29ms   370.70ms
    HTTP codes:
      1xx - 0, 2xx - 92605, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7395
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7395
    Throughput:     1.90MB/s
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
    Reqs/sec      6994.41    5138.35   65435.11
    Latency        7.14ms     3.98ms   363.90ms
    HTTP codes:
      1xx - 0, 2xx - 91810, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8190
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8190
    Throughput:     1.84MB/s
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
    Reqs/sec     23533.89    7089.16   36333.62
    Latency        2.12ms     1.93ms   171.67ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.34MB/s
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
    Reqs/sec     21201.37    5931.59   28933.77
    Latency        2.36ms     2.06ms   185.47ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.79MB/s
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
    Reqs/sec     62604.40    3443.88   65727.50
    Latency      796.46us    69.47us     2.70ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.89MB/s
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
    Reqs/sec     19181.66    7872.99   66163.52
    Latency        2.60ms     2.39ms   212.09ms
    HTTP codes:
      1xx - 0, 2xx - 93119, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6881
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6881
    Throughput:     4.04MB/s
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
    Reqs/sec     24536.02    8735.84   84984.88
    Latency        2.03ms     1.84ms   160.72ms
    HTTP codes:
      1xx - 0, 2xx - 95683, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4317
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4317
    Throughput:     5.38MB/s
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
    Reqs/sec     74983.46    2391.58   80810.91
    Latency      664.28us   182.40us     8.27ms
    HTTP codes:
      1xx - 0, 2xx - 96503, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3497
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3497
    Throughput:    11.45MB/s
  ```


