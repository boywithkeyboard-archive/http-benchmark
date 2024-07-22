## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74409` | `6708` | `79261` |
| **84%** | [Hyper Express](#hyper-express) | `62549` | `4175` | `81049` |
| **34%** | [Node (Default)](#node-default) | `25316` | `7294` | `43068` |
| **31%** | [Fastify](#fastify) | `23246` | `6807` | `35919` |
| **28%** | [Hono](#hono) | `20467` | `6044` | `29599` |
| **24%** | [Koa](#koa) | `17715` | `6361` | `61020` |
| **11%** | [Carbon](#carbon) | `8086` | `1356` | `10323` |
| **9%** | [Express](#express) | `6521` | `1014` | `8579` |


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
    Reqs/sec      8539.33    3739.40   59461.31
    Latency        5.84ms     4.16ms   359.71ms
    HTTP codes:
      1xx - 0, 2xx - 94997, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5003
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5003
    Throughput:     1.84MB/s
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
    Reqs/sec      6471.42    1009.06    8475.42
    Latency        7.72ms     3.57ms   342.82ms
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
    Reqs/sec     23929.72    7598.60   35449.98
    Latency        2.09ms     2.05ms   184.10ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.43MB/s
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
    Reqs/sec     20894.80    5702.37   30091.09
    Latency        2.39ms     2.06ms   183.10ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.72MB/s
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
    Reqs/sec     61721.15    3490.73   65880.36
    Latency      807.92us    64.41us     2.83ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.77MB/s
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
    Reqs/sec     17741.06    6236.71   59164.52
    Latency        2.81ms     2.37ms   208.26ms
    HTTP codes:
      1xx - 0, 2xx - 94111, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5889
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5889
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
    Reqs/sec     26379.79    8260.02   61345.54
    Latency        1.89ms     1.85ms   150.57ms
    HTTP codes:
      1xx - 0, 2xx - 97266, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2734
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2734
    Throughput:     5.87MB/s
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
    Reqs/sec     74026.33    6981.24   92737.15
    Latency      673.57us   204.37us    11.30ms
    HTTP codes:
      1xx - 0, 2xx - 97626, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2374
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2374
    Throughput:    11.43MB/s
  ```


