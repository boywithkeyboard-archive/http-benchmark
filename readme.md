## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `76508` | `6932` | `89177` |
| **83%** | [Hyper Express](#hyper-express) | `63810` | `4404` | `70028` |
| **34%** | [Node (Default)](#node-default) | `26308` | `6960` | `50763` |
| **32%** | [Fastify](#fastify) | `24811` | `7211` | `36896` |
| **28%** | [Hono](#hono) | `21201` | `5588` | `30024` |
| **25%** | [Koa](#koa) | `18853` | `6446` | `60750` |
| **11%** | [Carbon](#carbon) | `8501` | `1493` | `10687` |
| **9%** | [Express](#express) | `6768` | `1130` | `8735` |


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
    Reqs/sec      8881.25    4743.84   55080.25
    Latency        5.62ms     4.21ms   363.36ms
    HTTP codes:
      1xx - 0, 2xx - 92770, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7230
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7230
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
    Reqs/sec      7160.16    3787.95   50696.78
    Latency        6.98ms     3.80ms   347.44ms
    HTTP codes:
      1xx - 0, 2xx - 93631, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6369
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6368
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 1
    Throughput:     1.92MB/s
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
    Reqs/sec     23096.89    6550.72   35939.57
    Latency        2.16ms     2.02ms   182.40ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.24MB/s
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
    Reqs/sec     21927.58    6751.56   30706.69
    Latency        2.28ms     2.02ms   180.81ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.95MB/s
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
    Reqs/sec     65019.52    5390.01   87437.33
    Latency      766.95us    69.42us     4.29ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.23MB/s
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
    Reqs/sec     17908.53    5906.13   51824.44
    Latency        2.79ms     2.24ms   194.91ms
    HTTP codes:
      1xx - 0, 2xx - 94543, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5457
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5457
    Throughput:     3.83MB/s
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
    Reqs/sec     25736.09    7028.71   45046.17
    Latency        1.94ms     1.96ms   166.88ms
    HTTP codes:
      1xx - 0, 2xx - 98160, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1840
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1840
    Throughput:     5.78MB/s
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
    Reqs/sec     74473.76    5436.19   78964.83
    Latency      667.17us   187.68us    17.41ms
    HTTP codes:
      1xx - 0, 2xx - 97213, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2787
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2787
    Throughput:    11.47MB/s
  ```


