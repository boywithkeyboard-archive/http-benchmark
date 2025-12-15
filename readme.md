## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74761` | `2451` | `80109` |
| **83%** | [Hyper Express](#hyper-express) | `62343` | `3088` | `65741` |
| **32%** | [Node (Default)](#node-default) | `23999` | `7400` | `64195` |
| **31%** | [Fastify](#fastify) | `23170` | `7314` | `34293` |
| **28%** | [Hono](#hono) | `21073` | `6108` | `28770` |
| **25%** | [Koa](#koa) | `18960` | `8923` | `71474` |
| **10%** | [Carbon](#carbon) | `7684` | `1314` | `9972` |
| **8%** | [Express](#express) | `6028` | `998` | `7865` |


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
    Reqs/sec      8632.94    6423.24   81608.07
    Latency        5.78ms     4.38ms   378.11ms
    HTTP codes:
      1xx - 0, 2xx - 90450, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9550
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9550
    Throughput:     1.77MB/s
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
    Reqs/sec      6048.34     978.24    8043.85
    Latency        8.26ms     3.80ms   368.90ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.73MB/s
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
    Reqs/sec     24040.54    7277.68   34238.07
    Latency        2.08ms     2.09ms   186.13ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.45MB/s
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
    Reqs/sec     20090.52    5622.43   28146.82
    Latency        2.49ms     2.23ms   199.56ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.53MB/s
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
    Reqs/sec     62516.02    4118.52   67270.87
    Latency      797.44us    71.97us     3.51ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.88MB/s
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
    Reqs/sec     18486.01    8130.97   71146.89
    Latency        2.70ms     2.45ms   217.81ms
    HTTP codes:
      1xx - 0, 2xx - 92647, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7353
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7353
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
    Reqs/sec     23636.85    7179.05   63060.77
    Latency        2.11ms     2.06ms   178.25ms
    HTTP codes:
      1xx - 0, 2xx - 97365, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2635
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2635
    Throughput:     5.27MB/s
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
    Reqs/sec     74359.58    2983.30   84263.11
    Latency      669.08us   216.33us    13.23ms
    HTTP codes:
      1xx - 0, 2xx - 95622, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4378
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4378
    Throughput:    11.25MB/s
  ```


