## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73905` | `2969` | `81057` |
| **82%** | [Hyper Express](#hyper-express) | `60765` | `3228` | `63533` |
| **32%** | [Fastify](#fastify) | `23754` | `8083` | `36743` |
| **32%** | [Node (Default)](#node-default) | `23579` | `7469` | `75547` |
| **28%** | [Hono](#hono) | `20416` | `6264` | `29690` |
| **24%** | [Koa](#koa) | `17644` | `8151` | `67814` |
| **11%** | [Carbon](#carbon) | `8007` | `1475` | `10324` |
| **8%** | [Express](#express) | `6211` | `1026` | `8373` |


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
    Reqs/sec      8504.30    6299.95   78942.04
    Latency        5.87ms     4.35ms   373.86ms
    HTTP codes:
      1xx - 0, 2xx - 91027, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8973
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8973
    Throughput:     1.76MB/s
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
    Reqs/sec      5977.15     945.33    8181.66
    Latency        8.36ms     3.76ms   360.34ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.71MB/s
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
    Reqs/sec     24025.01    8051.11   36080.00
    Latency        2.08ms     1.98ms   176.27ms
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
    Reqs/sec     19908.83    5789.44   29546.06
    Latency        2.51ms     2.11ms   186.43ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.49MB/s
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
    Reqs/sec     60274.41    2907.41   62858.03
    Latency      827.32us    68.30us     3.36ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.56MB/s
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
    Reqs/sec     18160.24    8859.78   71248.60
    Latency        2.75ms     2.34ms   206.60ms
    HTTP codes:
      1xx - 0, 2xx - 91818, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8182
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8182
    Throughput:     3.77MB/s
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
    Reqs/sec     23351.45    7066.93   66517.83
    Latency        2.14ms     1.93ms   171.73ms
    HTTP codes:
      1xx - 0, 2xx - 97092, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2908
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2908
    Throughput:     5.19MB/s
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
    Reqs/sec     73304.43    3289.30   85272.37
    Latency      679.20us   147.70us     7.56ms
    HTTP codes:
      1xx - 0, 2xx - 97489, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2511
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2511
    Throughput:    11.32MB/s
  ```


