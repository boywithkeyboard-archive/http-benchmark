## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75339` | `6545` | `82872` |
| **82%** | [Hyper Express](#hyper-express) | `62021` | `3313` | `64672` |
| **34%** | [Node (Default)](#node-default) | `25967` | `7562` | `41414` |
| **31%** | [Fastify](#fastify) | `23411` | `6221` | `34499` |
| **26%** | [Hono](#hono) | `19652` | `5138` | `28164` |
| **24%** | [Koa](#koa) | `17807` | `6197` | `48240` |
| **11%** | [Carbon](#carbon) | `8243` | `1364` | `10336` |
| **9%** | [Express](#express) | `6412` | `1003` | `8419` |


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
    Reqs/sec      8901.94    5072.31   66709.41
    Latency        5.60ms     4.21ms   364.99ms
    HTTP codes:
      1xx - 0, 2xx - 92213, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7787
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7787
    Throughput:     1.87MB/s
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
    Reqs/sec      6537.89    1017.28    8434.02
    Latency        7.64ms     3.45ms   331.93ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.87MB/s
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
    Reqs/sec     24007.37    6918.07   34891.54
    Latency        2.08ms     1.98ms   178.63ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.44MB/s
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
    Reqs/sec     19910.00    5045.97   28081.17
    Latency        2.51ms     1.85ms   168.96ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.50MB/s
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
    Reqs/sec     62273.32    3824.22   72712.30
    Latency      800.02us    79.66us     4.59ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.85MB/s
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
    Reqs/sec     18182.91    6658.30   53883.77
    Latency        2.74ms     2.37ms   204.33ms
    HTTP codes:
      1xx - 0, 2xx - 93728, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6272
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6272
    Throughput:     3.85MB/s
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
    Reqs/sec     24342.49    6375.18   39991.40
    Latency        2.05ms     1.88ms   156.77ms
    HTTP codes:
      1xx - 0, 2xx - 98364, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1636
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1636
    Throughput:     5.48MB/s
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
    Reqs/sec     74036.18    5609.59   78940.90
    Latency      672.32us   212.95us    10.55ms
    HTTP codes:
      1xx - 0, 2xx - 97636, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2364
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2364
    Throughput:    11.44MB/s
  ```


