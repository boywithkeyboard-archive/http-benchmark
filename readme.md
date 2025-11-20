## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74362` | `3236` | `85916` |
| **83%** | [Hyper Express](#hyper-express) | `61993` | `3568` | `66479` |
| **34%** | [Node (Default)](#node-default) | `24967` | `8228` | `76974` |
| **31%** | [Fastify](#fastify) | `22798` | `7100` | `35155` |
| **26%** | [Hono](#hono) | `19435` | `5433` | `29660` |
| **25%** | [Koa](#koa) | `18222` | `8776` | `75224` |
| **11%** | [Carbon](#carbon) | `8133` | `1511` | `10225` |
| **8%** | [Express](#express) | `6266` | `1080` | `8180` |


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
    Reqs/sec      8740.34    6400.78   65831.78
    Latency        5.71ms     4.48ms   386.44ms
    HTTP codes:
      1xx - 0, 2xx - 90174, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9826
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9826
    Throughput:     1.79MB/s
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
    Reqs/sec      6275.62    1065.95    8190.75
    Latency        7.96ms     3.65ms   350.71ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.80MB/s
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
    Reqs/sec     22374.95    6525.44   35364.03
    Latency        2.23ms     1.98ms   177.22ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.08MB/s
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
    Reqs/sec     20342.59    5923.81   28642.00
    Latency        2.45ms     2.12ms   190.43ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.60MB/s
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
    Reqs/sec     62055.49    5038.45   92716.15
    Latency      808.44us    71.86us     2.92ms
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
    Reqs/sec     17879.07    8096.32   77479.23
    Latency        2.79ms     2.47ms   221.30ms
    HTTP codes:
      1xx - 0, 2xx - 92310, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7690
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7690
    Throughput:     3.74MB/s
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
    Reqs/sec     23804.68    7654.23   68536.98
    Latency        2.09ms     1.87ms   161.40ms
    HTTP codes:
      1xx - 0, 2xx - 96765, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3235
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3235
    Throughput:     5.28MB/s
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
    Reqs/sec     74689.65    3243.99   84149.97
    Latency      667.13us   124.17us     5.34ms
    HTTP codes:
      1xx - 0, 2xx - 97238, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2762
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2762
    Throughput:    11.49MB/s
  ```


