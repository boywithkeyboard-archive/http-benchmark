## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74593` | `5279` | `85838` |
| **85%** | [Hyper Express](#hyper-express) | `63111` | `3707` | `68555` |
| **35%** | [Node (Default)](#node-default) | `26124` | `7863` | `59547` |
| **34%** | [Fastify](#fastify) | `25518` | `7856` | `36131` |
| **28%** | [Hono](#hono) | `20529` | `5658` | `29925` |
| **25%** | [Koa](#koa) | `18327` | `6764` | `56472` |
| **11%** | [Carbon](#carbon) | `8056` | `1345` | `10224` |
| **9%** | [Express](#express) | `6478` | `1068` | `8671` |


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
    Reqs/sec      8669.85    4446.85   56172.59
    Latency        5.75ms     4.28ms   370.54ms
    HTTP codes:
      1xx - 0, 2xx - 93464, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6536
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6536
    Throughput:     1.84MB/s
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
    Reqs/sec      6454.26    1060.03    8370.17
    Latency        7.75ms     3.76ms   359.13ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     25249.18    7813.35   36152.87
    Latency        1.98ms     2.06ms   185.70ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.72MB/s
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
    Reqs/sec     20119.75    5813.25   29367.41
    Latency        2.48ms     2.05ms   185.63ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.54MB/s
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
    Reqs/sec     63461.43    4120.94   66869.59
    Latency      785.93us    76.43us     3.51ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.01MB/s
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
    Reqs/sec     18528.09    6692.26   54630.09
    Latency        2.69ms     2.33ms   203.26ms
    HTTP codes:
      1xx - 0, 2xx - 93385, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6615
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6615
    Throughput:     3.91MB/s
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
    Reqs/sec     25764.93    8014.00   51586.33
    Latency        1.94ms     1.90ms   167.61ms
    HTTP codes:
      1xx - 0, 2xx - 97041, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2959
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2959
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
    Reqs/sec     74466.97    5357.79   83578.55
    Latency      669.52us   161.08us     9.13ms
    HTTP codes:
      1xx - 0, 2xx - 97761, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2239
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2239
    Throughput:    11.51MB/s
  ```


