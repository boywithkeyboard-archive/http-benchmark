## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73599` | `5011` | `83358` |
| **83%** | [Hyper Express](#hyper-express) | `61359` | `3709` | `72154` |
| **34%** | [Node (Default)](#node-default) | `25029` | `6494` | `49839` |
| **31%** | [Fastify](#fastify) | `22493` | `6189` | `36464` |
| **25%** | [Hono](#hono) | `18086` | `3792` | `28486` |
| **24%** | [Koa](#koa) | `17882` | `6711` | `58301` |
| **11%** | [Carbon](#carbon) | `8192` | `1384` | `10226` |
| **9%** | [Express](#express) | `6510` | `1104` | `8419` |


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
    Reqs/sec      8470.09    4370.34   54452.50
    Latency        5.90ms     4.33ms   374.14ms
    HTTP codes:
      1xx - 0, 2xx - 93939, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6061
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6061
    Throughput:     1.81MB/s
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
    Reqs/sec      6472.15    1075.69    8270.62
    Latency        7.72ms     3.73ms   356.60ms
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
    Reqs/sec     22729.99    5827.89   37221.07
    Latency        2.20ms     2.01ms   177.75ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.16MB/s
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
    Reqs/sec     18416.88    3829.65   28723.04
    Latency        2.71ms     1.97ms   178.38ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.16MB/s
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
    Reqs/sec     61430.38    3208.96   64094.10
    Latency      811.04us    68.40us     3.20ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.73MB/s
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
    Reqs/sec     16578.18    6051.77   61530.98
    Latency        3.01ms     2.34ms   204.91ms
    HTTP codes:
      1xx - 0, 2xx - 93256, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6744
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6744
    Throughput:     3.50MB/s
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
    Reqs/sec     25830.74    7659.48   56397.18
    Latency        1.93ms     1.89ms   161.00ms
    HTTP codes:
      1xx - 0, 2xx - 96968, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3032
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3032
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
    Reqs/sec     73607.25    5280.95   87907.52
    Latency      679.06us   162.63us     6.45ms
    HTTP codes:
      1xx - 0, 2xx - 97698, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2302
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2302
    Throughput:    11.35MB/s
  ```


