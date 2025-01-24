## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74069` | `5476` | `85458` |
| **86%** | [Hyper Express](#hyper-express) | `63812` | `3372` | `70893` |
| **36%** | [Node (Default)](#node-default) | `26716` | `8184` | `54930` |
| **33%** | [Fastify](#fastify) | `24595` | `7829` | `36143` |
| **28%** | [Hono](#hono) | `20869` | `5835` | `29568` |
| **25%** | [Koa](#koa) | `18590` | `6485` | `50076` |
| **11%** | [Carbon](#carbon) | `8154` | `1409` | `10306` |
| **9%** | [Express](#express) | `6349` | `1044` | `8469` |


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
    Reqs/sec      8711.86    4968.01   57298.91
    Latency        5.73ms     4.33ms   376.96ms
    HTTP codes:
      1xx - 0, 2xx - 92580, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7420
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7420
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
    Reqs/sec      6900.56    4590.62   57176.63
    Latency        7.23ms     4.03ms   364.25ms
    HTTP codes:
      1xx - 0, 2xx - 91327, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8673
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8673
    Throughput:     1.81MB/s
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
    Reqs/sec     24301.02    7736.43   35532.98
    Latency        2.06ms     2.08ms   186.15ms
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
    Reqs/sec     20504.82    5830.82   29609.95
    Latency        2.44ms     2.08ms   188.49ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.63MB/s
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
    Reqs/sec     63707.69    3831.30   68458.53
    Latency      782.76us    64.06us     3.32ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.05MB/s
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
    Reqs/sec     19102.21    6914.06   54096.82
    Latency        2.61ms     2.36ms   207.11ms
    HTTP codes:
      1xx - 0, 2xx - 93309, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6691
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6691
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
    Reqs/sec     25489.11    7381.28   57316.98
    Latency        1.96ms     1.96ms   167.12ms
    HTTP codes:
      1xx - 0, 2xx - 96964, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3036
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3036
    Throughput:     5.66MB/s
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
    Reqs/sec     74592.18    5343.85   89078.02
    Latency      667.75us   195.43us     8.02ms
    HTTP codes:
      1xx - 0, 2xx - 96568, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3432
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3432
    Throughput:    11.40MB/s
  ```


