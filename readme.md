## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `79083` | `2620` | `85355` |
| **87%** | [Hyper Express](#hyper-express) | `69064` | `3439` | `72743` |
| **46%** | [Node (Default)](#node-default) | `36461` | `9792` | `79076` |
| **42%** | [Fastify](#fastify) | `32958` | `9581` | `51234` |
| **37%** | [Koa](#koa) | `29591` | `12406` | `74318` |
| **37%** | [Hono](#hono) | `29058` | `8138` | `48016` |
| **12%** | [Carbon](#carbon) | `9388` | `2332` | `13737` |
| **10%** | [Express](#express) | `7569` | `1730` | `10513` |


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
    Reqs/sec     11056.72    9259.02   87601.50
    Latency        4.52ms     4.42ms   374.33ms
    HTTP codes:
      1xx - 0, 2xx - 86948, 3xx - 0, 4xx - 0, 5xx - 0
      others - 13052
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 13052
    Throughput:     2.18MB/s
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
    Reqs/sec      8530.56    7150.15   75864.26
    Latency        5.85ms     3.75ms   348.93ms
    HTTP codes:
      1xx - 0, 2xx - 89242, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10758
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10758
    Throughput:     2.18MB/s
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
    Reqs/sec     34076.50    9465.49   52297.07
    Latency        1.47ms     1.72ms   155.65ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.72MB/s
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
    Reqs/sec     29378.13    8175.11   47115.18
    Latency        1.70ms     2.04ms   177.37ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     6.64MB/s
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
    Reqs/sec     69610.06    2782.93   72808.13
    Latency      716.09us    71.93us     2.52ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.89MB/s
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
    Reqs/sec     28979.30   12703.96   77148.58
    Latency        1.72ms     2.23ms   196.21ms
    HTTP codes:
      1xx - 0, 2xx - 92229, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7771
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7771
    Throughput:     6.04MB/s
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
    Reqs/sec     36881.76   11631.58   86251.44
    Latency        1.35ms     1.78ms   152.39ms
    HTTP codes:
      1xx - 0, 2xx - 93616, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6384
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6384
    Throughput:     7.91MB/s
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
    Reqs/sec     79999.11    3833.92   91443.52
    Latency      623.03us   219.45us    14.57ms
    HTTP codes:
      1xx - 0, 2xx - 96283, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3717
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3717
    Throughput:    12.19MB/s
  ```


