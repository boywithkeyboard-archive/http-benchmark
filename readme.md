## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73942` | `6570` | `89963` |
| **84%** | [Hyper Express](#hyper-express) | `62269` | `3211` | `65189` |
| **36%** | [Node (Default)](#node-default) | `26862` | `7791` | `60720` |
| **33%** | [Fastify](#fastify) | `24183` | `7230` | `37330` |
| **28%** | [Hono](#hono) | `20920` | `6029` | `30814` |
| **27%** | [Koa](#koa) | `19817` | `7376` | `53850` |
| **11%** | [Carbon](#carbon) | `8247` | `1374` | `10455` |
| **9%** | [Express](#express) | `6543` | `1079` | `8559` |


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
    Reqs/sec      8677.95    3873.02   52499.84
    Latency        5.75ms     4.28ms   369.96ms
    HTTP codes:
      1xx - 0, 2xx - 94730, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5270
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5270
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
    Reqs/sec      6853.32    4334.06   50041.80
    Latency        7.28ms     3.82ms   347.81ms
    HTTP codes:
      1xx - 0, 2xx - 92110, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7890
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7890
    Throughput:     1.81MB/s
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
    Reqs/sec     24951.00    7631.56   36401.46
    Latency        2.00ms     2.04ms   185.22ms
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
    Reqs/sec     20779.13    5663.41   30741.03
    Latency        2.40ms     1.92ms   176.80ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.70MB/s
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
    Reqs/sec     62458.21    3227.26   66969.52
    Latency      799.11us    65.65us     3.32ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.87MB/s
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
    Reqs/sec     19013.25    7233.66   58543.37
    Latency        2.62ms     2.38ms   205.74ms
    HTTP codes:
      1xx - 0, 2xx - 92937, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7063
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7063
    Throughput:     3.99MB/s
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
    Reqs/sec     26471.68    7527.02   50501.09
    Latency        1.89ms     1.91ms   164.27ms
    HTTP codes:
      1xx - 0, 2xx - 97369, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2631
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2631
    Throughput:     5.90MB/s
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
    Reqs/sec     74940.57    5712.97   82957.41
    Latency      664.89us   161.68us     6.68ms
    HTTP codes:
      1xx - 0, 2xx - 97066, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2934
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2934
    Throughput:    11.50MB/s
  ```


