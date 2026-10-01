## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `158946` | `7349` | `168539` |
| **81%** | [Hyper Express](#hyper-express) | `128231` | `8011` | `134626` |
| **35%** | [Node (Default)](#node-default) | `55281` | `11890` | `129493` |
| **32%** | [Fastify](#fastify) | `50521` | `9151` | `60793` |
| **25%** | [Koa](#koa) | `40252` | `18452` | `145590` |
| **25%** | [Hono](#hono) | `40083` | `6968` | `46625` |
| **12%** | [Carbon](#carbon) | `19034` | `4525` | `26942` |
| **9%** | [Express](#express) | `13555` | `2333` | `18429` |


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
    Reqs/sec     21260.77   15985.86  144908.81
    Latency        2.35ms     3.21ms   261.93ms
    HTTP codes:
      1xx - 0, 2xx - 86743, 3xx - 0, 4xx - 0, 5xx - 0
      others - 13257
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 13257
    Throughput:     4.19MB/s
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
    Reqs/sec     14565.38   12464.12  137988.05
    Latency        3.43ms     2.49ms   220.72ms
    HTTP codes:
      1xx - 0, 2xx - 89162, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10838
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10838
    Throughput:     3.72MB/s
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
    Reqs/sec     54673.62   24715.29  147858.86
    Latency        0.91ms     1.08ms    86.40ms
    HTTP codes:
      1xx - 0, 2xx - 78003, 3xx - 0, 4xx - 0, 5xx - 0
      others - 21997
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 21997
    Throughput:     9.71MB/s
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
    Reqs/sec     45828.69   21170.73  141310.67
    Latency        1.08ms     1.10ms    88.92ms
    HTTP codes:
      1xx - 0, 2xx - 86159, 3xx - 0, 4xx - 0, 5xx - 0
      others - 13841
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 13841
    Throughput:     8.97MB/s
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
    Reqs/sec    129297.41    9144.07  154000.16
    Latency      384.73us   265.85us     9.16ms
    HTTP codes:
      1xx - 0, 2xx - 88898, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11102
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11102
    Throughput:    16.28MB/s
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
    Reqs/sec     38905.81   16560.65  135799.55
    Latency        1.28ms     1.12ms    95.02ms
    HTTP codes:
      1xx - 0, 2xx - 90041, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9959
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9959
    Throughput:     7.93MB/s
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
    Reqs/sec     54698.45   11912.85  130602.30
    Latency        0.91ms     0.94ms    70.58ms
    HTTP codes:
      1xx - 0, 2xx - 96064, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3936
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3936
    Throughput:    12.04MB/s
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
    Reqs/sec    155078.51   11483.32  166437.90
    Latency      319.23us   138.69us     9.46ms
    HTTP codes:
      1xx - 0, 2xx - 96026, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3974
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3974
    Throughput:    23.60MB/s
  ```


