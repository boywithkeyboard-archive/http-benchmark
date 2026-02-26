## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `70475` | `3285` | `77817` |
| **84%** | [Hyper Express](#hyper-express) | `59292` | `4123` | `73265` |
| **34%** | [Node (Default)](#node-default) | `23620` | `6387` | `62390` |
| **33%** | [Fastify](#fastify) | `23325` | `7566` | `35620` |
| **29%** | [Hono](#hono) | `20426` | `6260` | `30860` |
| **26%** | [Koa](#koa) | `18272` | `8443` | `76924` |
| **11%** | [Carbon](#carbon) | `7986` | `1434` | `10003` |
| **9%** | [Express](#express) | `6352` | `1100` | `8276` |


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
    Reqs/sec      8768.39    5866.19   72188.78
    Latency        5.69ms     4.45ms   383.86ms
    HTTP codes:
      1xx - 0, 2xx - 91120, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8880
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8880
    Throughput:     1.82MB/s
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
    Reqs/sec      6211.60    1074.13    8288.97
    Latency        8.05ms     3.84ms   369.37ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.78MB/s
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
    Reqs/sec     22577.29    7297.78   36215.38
    Latency        2.21ms     2.12ms   191.76ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.13MB/s
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
    Reqs/sec     20723.82    6417.84   30040.76
    Latency        2.41ms     2.13ms   191.78ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.68MB/s
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
    Reqs/sec     58690.02    3827.58   64970.83
    Latency        0.85ms    84.51us     3.09ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.33MB/s
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
    Reqs/sec     18656.27    8295.89   65314.01
    Latency        2.67ms     2.58ms   228.23ms
    HTTP codes:
      1xx - 0, 2xx - 92302, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7698
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7698
    Throughput:     3.89MB/s
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
    Reqs/sec     23482.68    6726.75   59519.71
    Latency        2.13ms     2.04ms   177.10ms
    HTTP codes:
      1xx - 0, 2xx - 97522, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2478
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2478
    Throughput:     5.24MB/s
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
    Reqs/sec     70787.32    4128.95   80262.62
    Latency      703.40us   157.89us     7.66ms
    HTTP codes:
      1xx - 0, 2xx - 96834, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3166
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3166
    Throughput:    10.85MB/s
  ```


