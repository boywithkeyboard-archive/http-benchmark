## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `79786` | `2942` | `84493` |
| **88%** | [Hyper Express](#hyper-express) | `70483` | `2984` | `73715` |
| **44%** | [Node (Default)](#node-default) | `35019` | `9498` | `82148` |
| **41%** | [Fastify](#fastify) | `32378` | `8970` | `51335` |
| **37%** | [Hono](#hono) | `29745` | `9052` | `46942` |
| **37%** | [Koa](#koa) | `29139` | `12912` | `89883` |
| **12%** | [Carbon](#carbon) | `9498` | `2339` | `13339` |
| **9%** | [Express](#express) | `7471` | `1726` | `10573` |


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
    Reqs/sec     10322.00    7199.46   88268.33
    Latency        4.84ms     4.26ms   368.11ms
    HTTP codes:
      1xx - 0, 2xx - 91343, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8657
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8657
    Throughput:     2.14MB/s
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
    Reqs/sec      8119.89    6154.30   77029.71
    Latency        6.15ms     3.76ms   344.36ms
    HTTP codes:
      1xx - 0, 2xx - 91152, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8848
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8848
    Throughput:     2.12MB/s
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
    Reqs/sec     36897.41   12038.57   51499.99
    Latency        1.36ms     2.00ms   173.29ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.36MB/s
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
    Reqs/sec     30694.76    9410.75   46201.68
    Latency        1.63ms     2.03ms   174.88ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     6.93MB/s
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
    Reqs/sec     69715.60    3298.29   72600.65
    Latency      715.39us    73.36us     4.06ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.90MB/s
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
    Reqs/sec     30607.48   14003.54   85194.28
    Latency        1.63ms     2.26ms   196.93ms
    HTTP codes:
      1xx - 0, 2xx - 90444, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9556
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9556
    Throughput:     6.26MB/s
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
    Reqs/sec     35873.74   11224.00   85167.73
    Latency        1.39ms     1.75ms   150.03ms
    HTTP codes:
      1xx - 0, 2xx - 93403, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6597
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6597
    Throughput:     7.66MB/s
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
    Reqs/sec     79996.26    2648.15   87784.20
    Latency      622.48us   175.26us    10.91ms
    HTTP codes:
      1xx - 0, 2xx - 95625, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4375
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4375
    Throughput:    12.11MB/s
  ```


