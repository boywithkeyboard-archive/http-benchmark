## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `69102` | `6465` | `116469` |
| **86%** | [Hyper Express](#hyper-express) | `59204` | `11811` | `157094` |
| **32%** | [Hono](#hono) | `21780` | `6930` | `31334` |
| **30%** | [Node (Default)](#node-default) | `20670` | `5711` | `62521` |
| **29%** | [Fastify](#fastify) | `20097` | `4810` | `36519` |
| **26%** | [Koa](#koa) | `18182` | `7853` | `60362` |
| **11%** | [Carbon](#carbon) | `7384` | `1276` | `10392` |
| **9%** | [Express](#express) | `6183` | `1091` | `8284` |


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
    Reqs/sec      8316.90    5458.54   63749.74
    Latency        6.00ms     4.61ms   391.39ms
    HTTP codes:
      1xx - 0, 2xx - 91861, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8139
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8139
    Throughput:     1.74MB/s
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
    Reqs/sec      6112.30    1069.68    8292.49
    Latency        8.18ms     3.85ms   367.51ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.75MB/s
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
    Reqs/sec     21237.04    6568.92   35695.03
    Latency        2.35ms     2.22ms   196.52ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.82MB/s
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
    Reqs/sec     21493.34    6671.96   30139.52
    Latency        2.33ms     2.16ms   189.28ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.85MB/s
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
    Reqs/sec     57953.22    3323.64   66116.88
    Latency        0.86ms   100.58us     4.40ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.23MB/s
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
    Reqs/sec     19943.81    9342.68   75090.96
    Latency        2.50ms     2.47ms   215.26ms
    HTTP codes:
      1xx - 0, 2xx - 91402, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8598
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8598
    Throughput:     4.13MB/s
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
    Reqs/sec     20621.96    5348.93   64450.62
    Latency        2.42ms     2.12ms   185.56ms
    HTTP codes:
      1xx - 0, 2xx - 97372, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2628
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2628
    Throughput:     4.60MB/s
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
    Reqs/sec     68831.80    3164.60   77801.72
    Latency      723.58us   183.89us     8.47ms
    HTTP codes:
      1xx - 0, 2xx - 95764, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4236
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4236
    Throughput:    10.43MB/s
  ```


