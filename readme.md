## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `121848` | `2711` | `133106` |
| **95%** | [Hyper Express](#hyper-express) | `115283` | `5140` | `122929` |
| **59%** | [Node (Default)](#node-default) | `71591` | `17243` | `125674` |
| **56%** | [Hono](#hono) | `68085` | `18294` | `80848` |
| **55%** | [Fastify](#fastify) | `66975` | `16869` | `85017` |
| **52%** | [Koa](#koa) | `63293` | `20289` | `124978` |
| **21%** | [Carbon](#carbon) | `25892` | `7359` | `37250` |
| **15%** | [Express](#express) | `17734` | `3637` | `24420` |


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
    Reqs/sec     27086.88   11276.77  102587.67
    Latency        1.84ms     3.06ms   252.30ms
    HTTP codes:
      1xx - 0, 2xx - 93526, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6474
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6474
    Throughput:     5.76MB/s
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
    Reqs/sec     20359.88   14028.72  129431.40
    Latency        2.45ms     2.13ms   190.09ms
    HTTP codes:
      1xx - 0, 2xx - 88533, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11467
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11467
    Throughput:     5.16MB/s
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
    Reqs/sec     75071.57   22206.76  118090.61
    Latency      662.34us     0.98ms    77.43ms
    HTTP codes:
      1xx - 0, 2xx - 82221, 3xx - 0, 4xx - 0, 5xx - 0
      others - 17779
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 17779
    Throughput:    14.03MB/s
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
    Reqs/sec     68484.69   20048.96  126966.45
    Latency      727.83us     0.95ms    75.31ms
    HTTP codes:
      1xx - 0, 2xx - 92584, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7416
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7416
    Throughput:    14.34MB/s
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
    Reqs/sec    115427.33    4429.94  123580.83
    Latency      431.19us   190.18us     8.19ms
    HTTP codes:
      1xx - 0, 2xx - 91115, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8885
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8885
    Throughput:    14.94MB/s
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
    Reqs/sec     61779.07   18930.71  133830.38
    Latency      805.39us     1.15ms    92.69ms
    HTTP codes:
      1xx - 0, 2xx - 90984, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9016
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9016
    Throughput:    12.73MB/s
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
    Reqs/sec     69445.61   17979.38  132388.38
    Latency      717.08us     0.85ms    61.48ms
    HTTP codes:
      1xx - 0, 2xx - 93807, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6193
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6193
    Throughput:    14.92MB/s
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
    Reqs/sec    121512.52    3520.22  126407.84
    Latency      409.84us   129.58us     7.68ms
    HTTP codes:
      1xx - 0, 2xx - 96435, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3565
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3565
    Throughput:    18.54MB/s
  ```


