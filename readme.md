## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74884` | `5743` | `98430` |
| **84%** | [Hyper Express](#hyper-express) | `62678` | `3536` | `67398` |
| **34%** | [Node (Default)](#node-default) | `25432` | `7389` | `44587` |
| **32%** | [Fastify](#fastify) | `23711` | `7194` | `37246` |
| **27%** | [Hono](#hono) | `20436` | `5497` | `29211` |
| **25%** | [Koa](#koa) | `19064` | `6003` | `49082` |
| **11%** | [Carbon](#carbon) | `8251` | `1371` | `10367` |
| **9%** | [Express](#express) | `6388` | `1007` | `8372` |


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
    Reqs/sec      8833.83    5209.33   59742.42
    Latency        5.65ms     4.31ms   375.68ms
    HTTP codes:
      1xx - 0, 2xx - 92036, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7964
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7964
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
    Reqs/sec      6867.02    4280.37   60132.38
    Latency        7.27ms     4.02ms   365.32ms
    HTTP codes:
      1xx - 0, 2xx - 92672, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7328
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7328
    Throughput:     1.82MB/s
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
    Reqs/sec     23455.64    6987.95   36405.71
    Latency        2.13ms     2.06ms   184.97ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.32MB/s
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
    Reqs/sec     20311.14    5401.42   29360.79
    Latency        2.46ms     2.04ms   182.00ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.59MB/s
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
    Reqs/sec     62177.95    3148.46   68397.56
    Latency      801.51us    70.55us     3.12ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.84MB/s
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
    Reqs/sec     18731.61    7443.26   58094.92
    Latency        2.66ms     2.35ms   201.59ms
    HTTP codes:
      1xx - 0, 2xx - 91890, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8110
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8110
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
    Reqs/sec     26235.16    7935.00   55774.43
    Latency        1.90ms     1.84ms   157.99ms
    HTTP codes:
      1xx - 0, 2xx - 96132, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3868
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3868
    Throughput:     5.77MB/s
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
    Reqs/sec     74086.16    4952.30   82770.95
    Latency      672.43us   142.38us     6.17ms
    HTTP codes:
      1xx - 0, 2xx - 97510, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2490
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2490
    Throughput:    11.43MB/s
  ```


