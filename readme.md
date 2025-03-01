## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74059` | `4946` | `83094` |
| **86%** | [Hyper Express](#hyper-express) | `63786` | `3888` | `70766` |
| **36%** | [Node (Default)](#node-default) | `26691` | `7582` | `59850` |
| **33%** | [Fastify](#fastify) | `24232` | `7043` | `36689` |
| **29%** | [Hono](#hono) | `21651` | `5818` | `30785` |
| **26%** | [Koa](#koa) | `19536` | `6998` | `50905` |
| **11%** | [Carbon](#carbon) | `8504` | `1463` | `10573` |
| **9%** | [Express](#express) | `6610` | `1065` | `8564` |


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
    Reqs/sec      8765.54    5197.41   63788.99
    Latency        5.70ms     4.13ms   357.76ms
    HTTP codes:
      1xx - 0, 2xx - 92192, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7808
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7808
    Throughput:     1.83MB/s
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
    Reqs/sec      7273.46    5128.96   63181.66
    Latency        6.86ms     3.84ms   348.45ms
    HTTP codes:
      1xx - 0, 2xx - 90475, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9525
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9524
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 1
    Throughput:     1.88MB/s
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
    Reqs/sec     25804.53    7692.11   37453.47
    Latency        1.94ms     1.81ms   165.64ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.85MB/s
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
    Reqs/sec     20848.55    5640.75   30329.74
    Latency        2.40ms     1.98ms   180.01ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.71MB/s
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
    Reqs/sec     62991.58    4328.76   67427.71
    Latency      791.22us    86.27us     3.00ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.95MB/s
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
    Reqs/sec     19686.61    6328.72   53029.77
    Latency        2.53ms     2.27ms   198.37ms
    HTTP codes:
      1xx - 0, 2xx - 94993, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5007
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5007
    Throughput:     4.23MB/s
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
    Reqs/sec     28211.46    8598.46   49714.86
    Latency        1.77ms     1.88ms   162.94ms
    HTTP codes:
      1xx - 0, 2xx - 98155, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1845
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1845
    Throughput:     6.34MB/s
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
    Reqs/sec     74965.71    5336.09   83132.87
    Latency      664.52us   171.71us     7.01ms
    HTTP codes:
      1xx - 0, 2xx - 96892, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3108
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3108
    Throughput:    11.50MB/s
  ```


