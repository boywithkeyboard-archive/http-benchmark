## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73499` | `5335` | `78885` |
| **84%** | [Hyper Express](#hyper-express) | `61684` | `3339` | `71459` |
| **35%** | [Node (Default)](#node-default) | `25382` | `7396` | `41562` |
| **33%** | [Fastify](#fastify) | `24304` | `6607` | `34448` |
| **28%** | [Hono](#hono) | `20933` | `5770` | `28689` |
| **26%** | [Koa](#koa) | `18929` | `7248` | `54817` |
| **11%** | [Carbon](#carbon) | `8208` | `1396` | `10386` |
| **9%** | [Express](#express) | `6289` | `987` | `8366` |


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
    Reqs/sec      8715.43    4552.09   57075.73
    Latency        5.73ms     4.29ms   370.68ms
    HTTP codes:
      1xx - 0, 2xx - 93476, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6524
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6524
    Throughput:     1.85MB/s
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
    Reqs/sec      6223.91    1013.14    8175.04
    Latency        8.03ms     3.72ms   358.75ms
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
    Reqs/sec     23508.42    6841.44   34310.11
    Latency        2.13ms     1.98ms   180.09ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.33MB/s
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
    Reqs/sec     20941.36    5693.98   29229.27
    Latency        2.39ms     1.93ms   174.79ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.73MB/s
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
    Reqs/sec     60463.75    3804.33   63433.87
    Latency      824.79us    73.67us     4.02ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.59MB/s
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
    Reqs/sec     18745.69    6287.65   43638.68
    Latency        2.66ms     2.32ms   203.95ms
    HTTP codes:
      1xx - 0, 2xx - 95082, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4918
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4918
    Throughput:     4.03MB/s
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
    Reqs/sec     26399.49    7653.84   38741.99
    Latency        1.89ms     2.01ms   172.43ms
    HTTP codes:
      1xx - 0, 2xx - 98272, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1728
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1728
    Throughput:     5.94MB/s
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
    Reqs/sec     77228.85    7180.22   86606.02
    Latency      644.96us   167.70us     8.68ms
    HTTP codes:
      1xx - 0, 2xx - 98279, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1721
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1721
    Throughput:    12.00MB/s
  ```


