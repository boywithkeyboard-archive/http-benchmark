## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `100573` | `3489` | `110586` |
| **90%** | [Hyper Express](#hyper-express) | `90226` | `4702` | `105861` |
| **52%** | [Node (Default)](#node-default) | `52335` | `16958` | `101735` |
| **44%** | [Fastify](#fastify) | `43846` | `14605` | `65324` |
| **40%** | [Hono](#hono) | `40394` | `13722` | `60240` |
| **37%** | [Koa](#koa) | `37453` | `16013` | `97227` |
| **11%** | [Carbon](#carbon) | `11403` | `2359` | `16988` |
| **9%** | [Express](#express) | `9485` | `1882` | `13794` |


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
    Reqs/sec     13179.08   10748.27  111716.30
    Latency        3.77ms     3.34ms   284.72ms
    HTTP codes:
      1xx - 0, 2xx - 87887, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12113
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12113
    Throughput:     2.64MB/s
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
    Reqs/sec     10384.50    7793.29   94080.23
    Latency        4.80ms     2.84ms   260.46ms
    HTTP codes:
      1xx - 0, 2xx - 91466, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8534
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8534
    Throughput:     2.72MB/s
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
    Reqs/sec     49605.72   16330.13   67083.26
    Latency        1.01ms     1.39ms   123.34ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    11.26MB/s
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
    Reqs/sec     42172.37   15484.76   59915.37
    Latency        1.18ms     1.58ms   139.14ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.55MB/s
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
    Reqs/sec     87409.84    4061.20   92055.27
    Latency      569.34us    42.99us     2.07ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    12.44MB/s
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
    Reqs/sec     37617.16   17550.00  113255.29
    Latency        1.32ms     1.73ms   148.85ms
    HTTP codes:
      1xx - 0, 2xx - 88202, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11798
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11798
    Throughput:     7.51MB/s
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
    Reqs/sec     46078.18   15225.81   91257.15
    Latency        1.08ms     1.24ms   109.58ms
    HTTP codes:
      1xx - 0, 2xx - 96812, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3188
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3188
    Throughput:    10.25MB/s
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
    Reqs/sec    100148.91    2761.28  104299.55
    Latency      497.06us   141.58us     6.68ms
    HTTP codes:
      1xx - 0, 2xx - 96594, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3406
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3406
    Throughput:    15.31MB/s
  ```


