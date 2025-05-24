## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74554` | `4358` | `84983` |
| **84%** | [Hyper Express](#hyper-express) | `62872` | `3507` | `65617` |
| **36%** | [Node (Default)](#node-default) | `26736` | `8298` | `59482` |
| **33%** | [Fastify](#fastify) | `24878` | `7944` | `36971` |
| **28%** | [Hono](#hono) | `20995` | `5904` | `30378` |
| **25%** | [Koa](#koa) | `18331` | `6902` | `57858` |
| **11%** | [Carbon](#carbon) | `8286` | `1360` | `10295` |
| **9%** | [Express](#express) | `6520` | `1056` | `8479` |


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
    Reqs/sec      8725.64    3616.74   48237.32
    Latency        5.72ms     4.21ms   366.19ms
    HTTP codes:
      1xx - 0, 2xx - 95254, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4746
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4746
    Throughput:     1.89MB/s
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
    Reqs/sec      6527.97    1072.18    8499.46
    Latency        7.66ms     3.69ms   353.90ms
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
    Reqs/sec     25086.32    8120.58   37638.23
    Latency        1.99ms     1.98ms   182.38ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.69MB/s
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
    Reqs/sec     20346.95    5733.02   30407.70
    Latency        2.46ms     2.06ms   184.16ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.60MB/s
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
    Reqs/sec     63202.34    3416.99   65976.48
    Latency      789.15us    71.24us     2.58ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.98MB/s
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
    Reqs/sec     19388.38    6562.37   48602.89
    Latency        2.57ms     2.39ms   209.89ms
    HTTP codes:
      1xx - 0, 2xx - 94897, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5103
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5103
    Throughput:     4.16MB/s
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
    Reqs/sec     26544.80    7425.96   44457.94
    Latency        1.88ms     1.91ms   166.29ms
    HTTP codes:
      1xx - 0, 2xx - 98186, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1814
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1814
    Throughput:     5.97MB/s
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
    Reqs/sec     72748.37    4864.43   76856.78
    Latency      684.74us   171.95us     9.18ms
    HTTP codes:
      1xx - 0, 2xx - 97788, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2212
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2212
    Throughput:    11.26MB/s
  ```


