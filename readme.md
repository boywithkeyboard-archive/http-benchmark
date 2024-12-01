## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74023` | `6387` | `91935` |
| **85%** | [Hyper Express](#hyper-express) | `62825` | `3974` | `70932` |
| **34%** | [Node (Default)](#node-default) | `25356` | `7361` | `53074` |
| **31%** | [Fastify](#fastify) | `22786` | `6127` | `34845` |
| **28%** | [Hono](#hono) | `20836` | `6231` | `30106` |
| **23%** | [Koa](#koa) | `16911` | `6160` | `58319` |
| **11%** | [Carbon](#carbon) | `8249` | `1402` | `10447` |
| **9%** | [Express](#express) | `6330` | `949` | `8386` |


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
    Reqs/sec      8450.43    3376.38   48133.42
    Latency        5.91ms     4.19ms   368.12ms
    HTTP codes:
      1xx - 0, 2xx - 95644, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4356
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4356
    Throughput:     1.83MB/s
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
    Reqs/sec      6554.57    1029.82    8570.99
    Latency        7.62ms     3.48ms   334.34ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.87MB/s
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
    Reqs/sec     24291.47    7476.86   35857.97
    Latency        2.06ms     2.04ms   184.42ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.50MB/s
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
    Reqs/sec     20822.13    5757.14   30080.67
    Latency        2.40ms     2.04ms   180.80ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.70MB/s
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
    Reqs/sec     61448.08    4123.45   66387.10
    Latency      808.64us    72.19us     3.25ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.76MB/s
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
    Reqs/sec     17870.14    6248.78   47990.10
    Latency        2.79ms     2.39ms   207.80ms
    HTTP codes:
      1xx - 0, 2xx - 94697, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5303
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5303
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
    Reqs/sec     26045.21    7905.54   44691.04
    Latency        1.92ms     1.88ms   163.42ms
    HTTP codes:
      1xx - 0, 2xx - 98211, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1789
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1789
    Throughput:     5.86MB/s
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
    Reqs/sec     74906.25    5250.48   92908.60
    Latency      663.42us   181.82us    26.60ms
    HTTP codes:
      1xx - 0, 2xx - 97161, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2839
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2839
    Throughput:    11.45MB/s
  ```


