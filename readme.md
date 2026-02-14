## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `70046` | `3055` | `77518` |
| **83%** | [Hyper Express](#hyper-express) | `58323` | `3563` | `63099` |
| **31%** | [Fastify](#fastify) | `21808` | `6082` | `36520` |
| **30%** | [Node (Default)](#node-default) | `21020` | `4861` | `58533` |
| **30%** | [Hono](#hono) | `20863` | `6563` | `30379` |
| **25%** | [Koa](#koa) | `17671` | `8529` | `79299` |
| **11%** | [Carbon](#carbon) | `7986` | `1528` | `10359` |
| **9%** | [Express](#express) | `6306` | `1167` | `8399` |


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
    Reqs/sec      8685.64    6142.93   75890.04
    Latency        5.73ms     4.50ms   382.35ms
    HTTP codes:
      1xx - 0, 2xx - 91061, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8939
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8939
    Throughput:     1.80MB/s
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
    Reqs/sec      6341.90    1187.91    8422.28
    Latency        7.88ms     3.90ms   372.37ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     20947.80    5393.10   36971.68
    Latency        2.38ms     2.07ms   185.58ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.75MB/s
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
    Reqs/sec     20962.82    6566.86   30469.80
    Latency        2.38ms     2.13ms   191.98ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.73MB/s
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
    Reqs/sec     58471.02    3446.32   63826.53
    Latency        0.85ms    90.70us     5.07ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.31MB/s
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
    Reqs/sec     18219.48    8588.51   75209.07
    Latency        2.74ms     2.48ms   218.86ms
    HTTP codes:
      1xx - 0, 2xx - 91830, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8170
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8170
    Throughput:     3.78MB/s
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
    Reqs/sec     22167.56    5031.42   52923.96
    Latency        2.25ms     1.91ms   166.44ms
    HTTP codes:
      1xx - 0, 2xx - 97729, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2271
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2271
    Throughput:     4.96MB/s
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
    Reqs/sec     69447.35    3104.73   77025.00
    Latency      717.14us   139.13us     5.68ms
    HTTP codes:
      1xx - 0, 2xx - 97505, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2495
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2495
    Throughput:    10.71MB/s
  ```


