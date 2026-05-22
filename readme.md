## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74629` | `4941` | `91774` |
| **81%** | [Hyper Express](#hyper-express) | `60822` | `3564` | `66060` |
| **29%** | [Hono](#hono) | `21997` | `6790` | `32247` |
| **28%** | [Fastify](#fastify) | `21022` | `6058` | `37817` |
| **28%** | [Node (Default)](#node-default) | `20645` | `5998` | `69971` |
| **23%** | [Koa](#koa) | `16978` | `7456` | `60659` |
| **10%** | [Carbon](#carbon) | `7598` | `1343` | `10541` |
| **8%** | [Express](#express) | `6310` | `1142` | `8361` |


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
    Reqs/sec      8264.17    5637.70   78575.66
    Latency        6.04ms     4.29ms   371.98ms
    HTTP codes:
      1xx - 0, 2xx - 92077, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7923
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7923
    Throughput:     1.73MB/s
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
    Reqs/sec      6240.06    1067.59    8401.70
    Latency        8.01ms     3.79ms   365.59ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.79MB/s
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
    Reqs/sec     21886.42    6802.98   37758.71
    Latency        2.28ms     1.96ms   179.99ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.97MB/s
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
    Reqs/sec     22073.64    6528.08   31116.14
    Latency        2.26ms     1.95ms   177.50ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.98MB/s
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
    Reqs/sec     60984.46    3659.91   68304.60
    Latency      817.60us    84.43us     3.05ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.66MB/s
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
    Reqs/sec     17851.17    8530.22   67807.48
    Latency        2.79ms     2.41ms   209.87ms
    HTTP codes:
      1xx - 0, 2xx - 92571, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7429
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7429
    Throughput:     3.74MB/s
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
    Reqs/sec     20331.61    5653.09   68667.26
    Latency        2.45ms     1.96ms   169.49ms
    HTTP codes:
      1xx - 0, 2xx - 96199, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3801
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3801
    Throughput:     4.48MB/s
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
    Reqs/sec     73666.11    3199.33   83173.08
    Latency      675.78us   178.50us     8.05ms
    HTTP codes:
      1xx - 0, 2xx - 96668, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3332
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3332
    Throughput:    11.27MB/s
  ```


