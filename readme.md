## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73604` | `5205` | `84128` |
| **85%** | [Hyper Express](#hyper-express) | `62696` | `4055` | `71813` |
| **37%** | [Node (Default)](#node-default) | `27015` | `8232` | `60197` |
| **33%** | [Fastify](#fastify) | `24081` | `7199` | `36053` |
| **29%** | [Hono](#hono) | `21333` | `5715` | `30603` |
| **26%** | [Koa](#koa) | `19232` | `6942` | `56143` |
| **12%** | [Carbon](#carbon) | `8491` | `1456` | `10480` |
| **9%** | [Express](#express) | `6393` | `993` | `8350` |


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
    Reqs/sec      8665.40    4456.88   50759.71
    Latency        5.76ms     4.38ms   375.00ms
    HTTP codes:
      1xx - 0, 2xx - 93249, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6751
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6751
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
    Reqs/sec      7174.28    5485.01   55654.16
    Latency        6.96ms     3.98ms   361.89ms
    HTTP codes:
      1xx - 0, 2xx - 88969, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11031
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11031
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
    Reqs/sec     24938.49    7408.94   36068.53
    Latency        2.00ms     2.06ms   182.37ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.66MB/s
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
    Reqs/sec     20928.58    5666.50   29880.59
    Latency        2.39ms     2.01ms   182.32ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.73MB/s
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
    Reqs/sec     63831.20    4288.74   70635.24
    Latency      780.57us    74.60us     3.57ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.07MB/s
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
    Reqs/sec     19229.54    6398.17   60504.27
    Latency        2.59ms     2.38ms   210.78ms
    HTTP codes:
      1xx - 0, 2xx - 94436, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5564
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5564
    Throughput:     4.11MB/s
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
    Reqs/sec     25649.06    7580.65   49285.15
    Latency        1.95ms     1.93ms   164.70ms
    HTTP codes:
      1xx - 0, 2xx - 97774, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2226
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2226
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
    Reqs/sec     73248.54    5349.36   85643.41
    Latency      680.13us   214.83us    15.77ms
    HTTP codes:
      1xx - 0, 2xx - 97676, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2324
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2324
    Throughput:    11.32MB/s
  ```


