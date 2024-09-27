## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `77417` | `5212` | `90697` |
| **81%** | [Hyper Express](#hyper-express) | `62861` | `4051` | `68909` |
| **35%** | [Node (Default)](#node-default) | `27049` | `8252` | `58915` |
| **33%** | [Fastify](#fastify) | `25488` | `7729` | `36806` |
| **28%** | [Hono](#hono) | `22056` | `6294` | `30380` |
| **25%** | [Koa](#koa) | `19701` | `7014` | `54661` |
| **11%** | [Carbon](#carbon) | `8481` | `1474` | `10590` |
| **8%** | [Express](#express) | `6555` | `1076` | `8341` |


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
    Reqs/sec      8945.61    5127.16   68756.62
    Latency        5.58ms     4.26ms   365.82ms
    HTTP codes:
      1xx - 0, 2xx - 92841, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7159
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7159
    Throughput:     1.88MB/s
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
    Reqs/sec      7072.25    4648.33   59035.68
    Latency        7.06ms     3.88ms   354.51ms
    HTTP codes:
      1xx - 0, 2xx - 91886, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8114
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8114
    Throughput:     1.86MB/s
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
    Reqs/sec     24999.87    7851.64   37333.98
    Latency        2.00ms     2.09ms   188.54ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.67MB/s
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
    Reqs/sec     21427.27    5798.46   30338.59
    Latency        2.33ms     2.05ms   184.69ms
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
    Reqs/sec     63426.78    4446.70   79265.76
    Latency      788.54us    75.01us     3.10ms
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
    Reqs/sec     19368.46    6859.86   54113.23
    Latency        2.58ms     2.36ms   207.21ms
    HTTP codes:
      1xx - 0, 2xx - 94359, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5641
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5641
    Throughput:     4.13MB/s
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
    Reqs/sec     28009.35    8105.01   51900.80
    Latency        1.78ms     1.78ms   152.06ms
    HTTP codes:
      1xx - 0, 2xx - 97655, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2345
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2345
    Throughput:     6.27MB/s
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
    Reqs/sec     76949.95    5890.78   93725.24
    Latency      647.79us   159.86us     5.64ms
    HTTP codes:
      1xx - 0, 2xx - 97582, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2418
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2418
    Throughput:    11.88MB/s
  ```


