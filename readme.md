## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73979` | `2320` | `82603` |
| **84%** | [Hyper Express](#hyper-express) | `62026` | `2920` | `64169` |
| **36%** | [Node (Default)](#node-default) | `26506` | `8430` | `72451` |
| **32%** | [Fastify](#fastify) | `23634` | `7105` | `36806` |
| **29%** | [Hono](#hono) | `21203` | `6437` | `30968` |
| **25%** | [Koa](#koa) | `18546` | `9015` | `77793` |
| **11%** | [Carbon](#carbon) | `8109` | `1436` | `10372` |
| **9%** | [Express](#express) | `6460` | `1096` | `8498` |


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
    Reqs/sec      9233.56    7437.58   78409.98
    Latency        5.40ms     4.30ms   371.94ms
    HTTP codes:
      1xx - 0, 2xx - 88941, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11059
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11059
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
    Reqs/sec      7150.92    6481.50   74090.99
    Latency        6.97ms     3.98ms   365.00ms
    HTTP codes:
      1xx - 0, 2xx - 88559, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11441
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11441
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
    Reqs/sec     24291.48    7507.72   37573.15
    Latency        2.06ms     2.01ms   179.00ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.51MB/s
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
    Reqs/sec     20510.74    5819.55   30989.21
    Latency        2.43ms     2.01ms   182.71ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.64MB/s
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
    Reqs/sec     62066.05    3329.60   64970.94
    Latency      803.03us    68.06us     2.63ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.82MB/s
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
    Reqs/sec     19677.16    9025.73   71669.20
    Latency        2.54ms     2.41ms   212.24ms
    HTTP codes:
      1xx - 0, 2xx - 91242, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8758
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8758
    Throughput:     4.06MB/s
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
    Reqs/sec     26416.72    9079.62   76274.74
    Latency        1.89ms     1.91ms   164.54ms
    HTTP codes:
      1xx - 0, 2xx - 96591, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3409
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3409
    Throughput:     5.84MB/s
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
    Reqs/sec     73932.68    3529.16   82641.90
    Latency      674.12us   135.76us     5.17ms
    HTTP codes:
      1xx - 0, 2xx - 97645, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2355
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2355
    Throughput:    11.43MB/s
  ```


