## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `70090` | `4690` | `84722` |
| **82%** | [Hyper Express](#hyper-express) | `57727` | `4125` | `66801` |
| **35%** | [Node (Default)](#node-default) | `24848` | `8013` | `60774` |
| **34%** | [Fastify](#fastify) | `23761` | `7351` | `33958` |
| **28%** | [Koa](#koa) | `19292` | `9992` | `75433` |
| **27%** | [Hono](#hono) | `19015` | `6047` | `28511` |
| **11%** | [Carbon](#carbon) | `7745` | `1232` | `10801` |
| **9%** | [Express](#express) | `6349` | `1057` | `8196` |


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
    Reqs/sec      8110.54    6073.01   73894.75
    Latency        6.15ms     4.72ms   400.64ms
    HTTP codes:
      1xx - 0, 2xx - 90423, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9577
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9577
    Throughput:     1.67MB/s
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
    Reqs/sec      6304.43    1056.65    8547.11
    Latency        7.93ms     3.83ms   366.66ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.80MB/s
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
    Reqs/sec     23897.66    7275.08   33495.30
    Latency        2.09ms     2.04ms   180.69ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.42MB/s
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
    Reqs/sec     20341.63    5894.04   27673.99
    Latency        2.46ms     2.20ms   196.52ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.59MB/s
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
    Reqs/sec     58910.19    3895.25   67157.00
    Latency      846.25us    96.60us     3.52ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.37MB/s
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
    Reqs/sec     17907.47    9246.55   73481.02
    Latency        2.79ms     2.46ms   213.08ms
    HTTP codes:
      1xx - 0, 2xx - 89593, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10407
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10407
    Throughput:     3.63MB/s
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
    Reqs/sec     24642.86    8534.05   72387.98
    Latency        2.02ms     1.95ms   170.23ms
    HTTP codes:
      1xx - 0, 2xx - 96121, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3879
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3879
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
    Reqs/sec     70247.12    3696.04   80861.26
    Latency      708.15us   184.19us    10.36ms
    HTTP codes:
      1xx - 0, 2xx - 95094, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4906
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4906
    Throughput:    10.57MB/s
  ```


