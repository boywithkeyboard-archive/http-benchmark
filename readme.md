## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73988` | `6381` | `92265` |
| **85%** | [Hyper Express](#hyper-express) | `62930` | `3992` | `70059` |
| **34%** | [Node (Default)](#node-default) | `25310` | `6806` | `53261` |
| **34%** | [Fastify](#fastify) | `25059` | `7778` | `36614` |
| **29%** | [Hono](#hono) | `21609` | `6235` | `31005` |
| **26%** | [Koa](#koa) | `19332` | `6738` | `60035` |
| **11%** | [Carbon](#carbon) | `8484` | `1465` | `10624` |
| **9%** | [Express](#express) | `6545` | `1044` | `8478` |


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
    Reqs/sec      8887.09    3703.65   45440.31
    Latency        5.61ms     4.11ms   356.75ms
    HTTP codes:
      1xx - 0, 2xx - 95189, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4811
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4811
    Throughput:     1.92MB/s
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
    Reqs/sec      7128.86    4889.43   57504.91
    Latency        7.00ms     3.80ms   345.49ms
    HTTP codes:
      1xx - 0, 2xx - 90561, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9439
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9415
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 24
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
    Reqs/sec     24614.22    7414.11   37467.90
    Latency        2.03ms     1.94ms   174.41ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.59MB/s
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
    Reqs/sec     21533.30    6127.17   30553.68
    Latency        2.32ms     2.07ms   185.11ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.86MB/s
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
    Reqs/sec     63919.57    4931.91   83644.19
    Latency      780.12us    71.30us     3.67ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.08MB/s
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
    Reqs/sec     18429.43    6592.93   54655.62
    Latency        2.71ms     2.41ms   208.18ms
    HTTP codes:
      1xx - 0, 2xx - 94332, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5668
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5668
    Throughput:     3.93MB/s
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
    Reqs/sec     25837.96    7707.38   56165.23
    Latency        1.93ms     1.97ms   167.68ms
    HTTP codes:
      1xx - 0, 2xx - 96389, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3611
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3611
    Throughput:     5.72MB/s
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
    Reqs/sec     75108.00    5624.91   81754.48
    Latency      663.38us   179.25us     7.57ms
    HTTP codes:
      1xx - 0, 2xx - 96908, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3092
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3092
    Throughput:    11.51MB/s
  ```


