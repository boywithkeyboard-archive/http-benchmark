## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73972` | `4935` | `80980` |
| **82%** | [Hyper Express](#hyper-express) | `60778` | `3508` | `65279` |
| **35%** | [Node (Default)](#node-default) | `26246` | `7783` | `51946` |
| **31%** | [Fastify](#fastify) | `22880` | `6349` | `36128` |
| **28%** | [Hono](#hono) | `20969` | `5748` | `29918` |
| **26%** | [Koa](#koa) | `19234` | `7152` | `59350` |
| **11%** | [Carbon](#carbon) | `8041` | `1359` | `10174` |
| **9%** | [Express](#express) | `6362` | `1023` | `8394` |


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
    Reqs/sec      8622.04    4241.35   59907.31
    Latency        5.79ms     4.21ms   364.63ms
    HTTP codes:
      1xx - 0, 2xx - 93896, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6104
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6104
    Throughput:     1.84MB/s
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
    Reqs/sec      6468.91    1047.97    8428.63
    Latency        7.73ms     3.75ms   354.62ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     23450.31    6446.51   37075.94
    Latency        2.13ms     2.02ms   182.29ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.32MB/s
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
    Reqs/sec     20436.91    5703.64   29053.77
    Latency        2.44ms     2.08ms   187.22ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.62MB/s
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
    Reqs/sec     61783.74    3149.01   68572.23
    Latency      806.92us    68.41us     2.51ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.78MB/s
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
    Reqs/sec     18963.72    7192.71   58032.26
    Latency        2.63ms     2.46ms   213.03ms
    HTTP codes:
      1xx - 0, 2xx - 93198, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6802
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6802
    Throughput:     4.00MB/s
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
    Reqs/sec     27178.01    8938.26   62341.37
    Latency        1.84ms     1.90ms   160.70ms
    HTTP codes:
      1xx - 0, 2xx - 96618, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3382
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3382
    Throughput:     6.01MB/s
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
    Reqs/sec     73722.81    5610.59   85110.54
    Latency      675.70us   165.74us     6.26ms
    HTTP codes:
      1xx - 0, 2xx - 97095, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2905
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2905
    Throughput:    11.33MB/s
  ```


