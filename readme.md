## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74932` | `2962` | `78997` |
| **83%** | [Hyper Express](#hyper-express) | `62415` | `3660` | `64571` |
| **34%** | [Node (Default)](#node-default) | `25390` | `7881` | `62894` |
| **33%** | [Fastify](#fastify) | `24932` | `7524` | `35352` |
| **28%** | [Hono](#hono) | `20904` | `6034` | `30501` |
| **25%** | [Koa](#koa) | `18451` | `8101` | `67361` |
| **11%** | [Carbon](#carbon) | `8032` | `1399` | `10330` |
| **8%** | [Express](#express) | `6259` | `1032` | `8419` |


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
    Reqs/sec      8811.76    7190.03   77504.42
    Latency        5.64ms     4.43ms   379.21ms
    HTTP codes:
      1xx - 0, 2xx - 88614, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11386
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11386
    Throughput:     1.78MB/s
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
    Reqs/sec      6454.28    1103.54    8452.45
    Latency        7.74ms     3.78ms   362.27ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.85MB/s
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
    Reqs/sec     24077.52    7542.94   35839.80
    Latency        2.08ms     2.05ms   182.17ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.46MB/s
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
    Reqs/sec     21130.09    5704.93   29255.85
    Latency        2.36ms     1.87ms   170.56ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.77MB/s
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
    Reqs/sec     61889.85    3449.33   64208.79
    Latency      805.68us    77.91us     3.08ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.79MB/s
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
    Reqs/sec     18453.62    8014.74   65348.17
    Latency        2.70ms     2.46ms   217.75ms
    HTTP codes:
      1xx - 0, 2xx - 92629, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7371
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7371
    Throughput:     3.87MB/s
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
    Reqs/sec     26136.26    7901.86   61281.47
    Latency        1.91ms     1.98ms   173.40ms
    HTTP codes:
      1xx - 0, 2xx - 97482, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2518
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2518
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
    Reqs/sec     73186.81    2485.99   79164.19
    Latency      680.66us   142.67us     7.28ms
    HTTP codes:
      1xx - 0, 2xx - 96880, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3120
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3120
    Throughput:    11.22MB/s
  ```


