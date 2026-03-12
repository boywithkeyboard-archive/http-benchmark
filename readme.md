## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `71030` | `3268` | `79506` |
| **84%** | [Hyper Express](#hyper-express) | `59914` | `3647` | `66020` |
| **35%** | [Node (Default)](#node-default) | `24581` | `7483` | `72309` |
| **32%** | [Fastify](#fastify) | `23061` | `6997` | `37452` |
| **30%** | [Hono](#hono) | `21143` | `6312` | `31658` |
| **26%** | [Koa](#koa) | `18656` | `8160` | `72715` |
| **11%** | [Carbon](#carbon) | `8124` | `1433` | `10292` |
| **9%** | [Express](#express) | `6260` | `1053` | `8419` |


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
    Reqs/sec      8777.10    5399.59   73884.76
    Latency        5.68ms     4.33ms   373.47ms
    HTTP codes:
      1xx - 0, 2xx - 92615, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7385
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7385
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
    Reqs/sec      6564.86    1145.51    8500.31
    Latency        7.61ms     3.68ms   353.80ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.88MB/s
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
    Reqs/sec     23365.57    7348.85   37190.55
    Latency        2.14ms     2.11ms   189.21ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.30MB/s
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
    Reqs/sec     20868.13    6069.40   30827.75
    Latency        2.39ms     2.07ms   184.32ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.72MB/s
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
    Reqs/sec     59504.57    3391.56   65557.24
    Latency      838.91us    79.59us     4.40ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.45MB/s
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
    Reqs/sec     18306.89    7987.51   69892.63
    Latency        2.73ms     2.45ms   214.05ms
    HTTP codes:
      1xx - 0, 2xx - 92443, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7557
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7557
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
    Reqs/sec     24253.13    7137.17   70013.17
    Latency        2.06ms     1.92ms   168.24ms
    HTTP codes:
      1xx - 0, 2xx - 96614, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3386
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3386
    Throughput:     5.37MB/s
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
    Reqs/sec     71984.93    3534.20   80455.16
    Latency      691.91us   160.82us     8.49ms
    HTTP codes:
      1xx - 0, 2xx - 96661, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3339
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3339
    Throughput:    11.01MB/s
  ```


