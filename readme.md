## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `116295` | `12454` | `134148` |
| **89%** | [Hyper Express](#hyper-express) | `103560` | `7490` | `108325` |
| **32%** | [Node (Default)](#node-default) | `37585` | `11557` | `114303` |
| **31%** | [Fastify](#fastify) | `36547` | `8640` | `57458` |
| **27%** | [Koa](#koa) | `31609` | `16307` | `123117` |
| **27%** | [Hono](#hono) | `31557` | `6503` | `47387` |
| **11%** | [Carbon](#carbon) | `12233` | `2360` | `16779` |
| **8%** | [Express](#express) | `9092` | `1654` | `13598` |


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
    Reqs/sec     14251.68   13398.39  120782.45
    Latency        3.50ms     3.50ms   300.95ms
    HTTP codes:
      1xx - 0, 2xx - 85268, 3xx - 0, 4xx - 0, 5xx - 0
      others - 14732
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 14732
    Throughput:     2.76MB/s
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
    Reqs/sec     10280.66   10750.67  108507.28
    Latency        4.85ms     3.39ms   310.01ms
    HTTP codes:
      1xx - 0, 2xx - 86319, 3xx - 0, 4xx - 0, 5xx - 0
      others - 13681
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 13681
    Throughput:     2.54MB/s
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
    Reqs/sec     38257.97    8863.70   56049.68
    Latency        1.30ms     1.40ms   122.50ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.68MB/s
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
    Reqs/sec     31938.13    6655.68   48394.87
    Latency        1.56ms     1.39ms   123.05ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.21MB/s
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
    Reqs/sec    104086.87    8164.61  111364.50
    Latency      479.10us    60.14us     2.37ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    14.76MB/s
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
    Reqs/sec     32589.22   14787.94  107106.25
    Latency        1.53ms     1.79ms   151.39ms
    HTTP codes:
      1xx - 0, 2xx - 90305, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9695
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9695
    Throughput:     6.66MB/s
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
    Reqs/sec     39961.58   11971.80  109327.71
    Latency        1.25ms     1.33ms   111.13ms
    HTTP codes:
      1xx - 0, 2xx - 95676, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4324
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4324
    Throughput:     8.74MB/s
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
    Reqs/sec    128067.07    8623.27  153925.42
    Latency      387.97us   163.98us     8.48ms
    HTTP codes:
      1xx - 0, 2xx - 95667, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4333
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4333
    Throughput:    19.37MB/s
  ```


