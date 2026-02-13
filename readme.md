## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `70164` | `3929` | `85862` |
| **84%** | [Hyper Express](#hyper-express) | `58603` | `3748` | `66441` |
| **33%** | [Node (Default)](#node-default) | `23019` | `5565` | `62665` |
| **33%** | [Fastify](#fastify) | `23012` | `7373` | `36895` |
| **28%** | [Hono](#hono) | `19544` | `5459` | `30722` |
| **27%** | [Koa](#koa) | `18889` | `7769` | `63151` |
| **12%** | [Carbon](#carbon) | `8118` | `1483` | `10390` |
| **9%** | [Express](#express) | `6248` | `1102` | `8273` |


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
    Reqs/sec      8536.60    5619.16   73379.91
    Latency        5.85ms     4.42ms   380.22ms
    HTTP codes:
      1xx - 0, 2xx - 91941, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8059
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8059
    Throughput:     1.78MB/s
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
    Reqs/sec      6090.82    1007.65    8282.72
    Latency        8.21ms     3.84ms   366.92ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.74MB/s
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
    Reqs/sec     22048.60    6790.41   36422.28
    Latency        2.27ms     2.17ms   194.26ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.00MB/s
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
    Reqs/sec     21366.77    6891.68   30999.00
    Latency        2.34ms     2.10ms   191.85ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.82MB/s
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
    Reqs/sec     58401.44    3192.55   64075.61
    Latency        0.85ms    84.55us     2.94ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.29MB/s
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
    Reqs/sec     18301.42    8210.36   75516.07
    Latency        2.72ms     2.52ms   223.32ms
    HTTP codes:
      1xx - 0, 2xx - 92207, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7793
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7793
    Throughput:     3.82MB/s
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
    Reqs/sec     23162.20    5666.59   59572.17
    Latency        2.15ms     1.93ms   166.84ms
    HTTP codes:
      1xx - 0, 2xx - 97521, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2479
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2479
    Throughput:     5.17MB/s
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
    Reqs/sec     70545.98    2967.08   78173.23
    Latency      706.42us   136.02us     8.09ms
    HTTP codes:
      1xx - 0, 2xx - 97264, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2736
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2736
    Throughput:    10.86MB/s
  ```


