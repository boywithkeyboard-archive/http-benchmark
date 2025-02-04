## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72136` | `3786` | `81352` |
| **86%** | [Hyper Express](#hyper-express) | `61836` | `3627` | `67378` |
| **37%** | [Node (Default)](#node-default) | `26546` | `7548` | `50977` |
| **35%** | [Fastify](#fastify) | `25406` | `7930` | `37148` |
| **29%** | [Hono](#hono) | `21074` | `5535` | `29378` |
| **27%** | [Koa](#koa) | `19415` | `6999` | `58255` |
| **11%** | [Carbon](#carbon) | `8221` | `1365` | `10391` |
| **9%** | [Express](#express) | `6353` | `971` | `8386` |


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
    Reqs/sec      8614.02    3947.82   53822.21
    Latency        5.80ms     4.35ms   376.72ms
    HTTP codes:
      1xx - 0, 2xx - 94598, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5402
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5402
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
    Reqs/sec      6981.00    4361.83   57689.95
    Latency        7.15ms     3.90ms   358.41ms
    HTTP codes:
      1xx - 0, 2xx - 92199, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7801
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7801
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
    Reqs/sec     24770.65    7767.36   37227.53
    Latency        2.02ms     2.02ms   186.08ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.62MB/s
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
    Reqs/sec     21405.89    5850.49   29625.41
    Latency        2.33ms     2.07ms   183.70ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.84MB/s
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
    Reqs/sec     63292.83    3572.09   74125.37
    Latency      787.75us    73.12us     3.75ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.99MB/s
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
    Reqs/sec     19279.52    7089.08   59355.89
    Latency        2.59ms     2.34ms   203.13ms
    HTTP codes:
      1xx - 0, 2xx - 93756, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6244
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6244
    Throughput:     4.09MB/s
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
    Reqs/sec     28288.10    8628.49   60452.77
    Latency        1.76ms     1.88ms   161.15ms
    HTTP codes:
      1xx - 0, 2xx - 97164, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2836
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2836
    Throughput:     6.30MB/s
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
    Reqs/sec     72417.39    5153.24   83366.50
    Latency      687.89us   195.36us    10.70ms
    HTTP codes:
      1xx - 0, 2xx - 96579, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3421
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3421
    Throughput:    11.07MB/s
  ```


