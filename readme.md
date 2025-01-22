## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73563` | `5457` | `83614` |
| **85%** | [Hyper Express](#hyper-express) | `62223` | `2786` | `65874` |
| **37%** | [Node (Default)](#node-default) | `26855` | `8371` | `54400` |
| **33%** | [Fastify](#fastify) | `24526` | `7115` | `36773` |
| **30%** | [Hono](#hono) | `21797` | `6129` | `29839` |
| **26%** | [Koa](#koa) | `18785` | `6822` | `56612` |
| **11%** | [Carbon](#carbon) | `8256` | `1399` | `10395` |
| **9%** | [Express](#express) | `6472` | `1049` | `8447` |


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
    Reqs/sec      8703.91    4019.24   49627.18
    Latency        5.74ms     4.38ms   377.92ms
    HTTP codes:
      1xx - 0, 2xx - 94587, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5413
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5413
    Throughput:     1.87MB/s
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
    Reqs/sec      6395.03    1044.59    8407.13
    Latency        7.81ms     3.70ms   350.29ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.83MB/s
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
    Reqs/sec     24085.67    7203.06   36042.01
    Latency        2.07ms     1.97ms   178.54ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.47MB/s
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
    Reqs/sec     19350.29    4712.52   29182.50
    Latency        2.58ms     2.00ms   179.84ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.37MB/s
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
    Reqs/sec     63466.47    4306.98   70333.31
    Latency      785.51us    82.45us     3.62ms
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
    Reqs/sec     18467.42    7092.72   57206.05
    Latency        2.70ms     2.42ms   211.75ms
    HTTP codes:
      1xx - 0, 2xx - 92628, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7372
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7372
    Throughput:     3.87MB/s
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
    Reqs/sec     25901.91    7702.22   49801.50
    Latency        1.93ms     1.99ms   170.34ms
    HTTP codes:
      1xx - 0, 2xx - 98020, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1980
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1980
    Throughput:     5.80MB/s
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
    Reqs/sec     74090.71    6184.58   86937.67
    Latency      672.96us   161.96us     7.45ms
    HTTP codes:
      1xx - 0, 2xx - 98111, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1889
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1889
    Throughput:    11.49MB/s
  ```


