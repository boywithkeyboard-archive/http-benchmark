## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `82257` | `3208` | `90410` |
| **85%** | [Hyper Express](#hyper-express) | `70102` | `3008` | `72722` |
| **45%** | [Node (Default)](#node-default) | `36772` | `11555` | `72817` |
| **43%** | [Fastify](#fastify) | `35226` | `10287` | `51846` |
| **35%** | [Hono](#hono) | `28602` | `8284` | `46919` |
| **33%** | [Koa](#koa) | `27489` | `11345` | `81657` |
| **12%** | [Carbon](#carbon) | `9564` | `2356` | `13281` |
| **9%** | [Express](#express) | `7729` | `1810` | `10817` |


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
    Reqs/sec     10263.39    8679.24   85756.32
    Latency        4.86ms     4.17ms   356.92ms
    HTTP codes:
      1xx - 0, 2xx - 88156, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11844
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11844
    Throughput:     2.06MB/s
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
    Reqs/sec      8538.24    7136.15   80254.26
    Latency        5.85ms     3.62ms   331.83ms
    HTTP codes:
      1xx - 0, 2xx - 89518, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10482
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10482
    Throughput:     2.19MB/s
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
    Reqs/sec     33213.90    9007.68   51912.86
    Latency        1.50ms     1.97ms   174.10ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.54MB/s
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
    Reqs/sec     29346.47    7703.04   46767.71
    Latency        1.70ms     1.96ms   173.77ms
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
    Reqs/sec     70583.83    2988.60   73770.80
    Latency      706.56us    63.40us     2.88ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    10.03MB/s
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
    Reqs/sec     29074.28   12450.04   78057.81
    Latency        1.71ms     2.32ms   199.62ms
    HTTP codes:
      1xx - 0, 2xx - 92494, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7506
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7506
    Throughput:     6.09MB/s
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
    Reqs/sec     38616.34   12291.84   84093.27
    Latency        1.29ms     1.94ms   161.54ms
    HTTP codes:
      1xx - 0, 2xx - 96344, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3656
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3656
    Throughput:     8.51MB/s
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
    Reqs/sec     81510.30    4819.43   92470.78
    Latency      607.92us   160.95us     8.71ms
    HTTP codes:
      1xx - 0, 2xx - 96570, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3430
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3430
    Throughput:    12.52MB/s
  ```


