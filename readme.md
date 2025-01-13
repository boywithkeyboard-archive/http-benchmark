## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74095` | `5669` | `84969` |
| **85%** | [Hyper Express](#hyper-express) | `63062` | `3732` | `68650` |
| **32%** | [Fastify](#fastify) | `23987` | `7181` | `35369` |
| **31%** | [Node (Default)](#node-default) | `22657` | `5549` | `36987` |
| **27%** | [Hono](#hono) | `19984` | `5121` | `29235` |
| **24%** | [Koa](#koa) | `17736` | `5549` | `41308` |
| **11%** | [Carbon](#carbon) | `8122` | `1400` | `10572` |
| **9%** | [Express](#express) | `6548` | `1061` | `8549` |


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
    Reqs/sec      8568.69    3545.14   52263.00
    Latency        5.83ms     4.19ms   373.15ms
    HTTP codes:
      1xx - 0, 2xx - 94896, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5104
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5104
    Throughput:     1.85MB/s
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
    Reqs/sec      6410.13     989.97    8558.56
    Latency        7.80ms     3.54ms   340.89ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.83MB/s
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
    Reqs/sec     23645.98    6738.26   35346.22
    Latency        2.11ms     1.86ms   168.95ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.37MB/s
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
    Reqs/sec     20252.45    5398.55   28069.32
    Latency        2.47ms     1.93ms   175.85ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.58MB/s
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
    Reqs/sec     63254.17    4161.98   75344.49
    Latency      788.25us    68.28us     2.91ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.98MB/s
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
    Reqs/sec     18107.78    7208.01   59993.83
    Latency        2.75ms     2.37ms   205.05ms
    HTTP codes:
      1xx - 0, 2xx - 91366, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8634
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8634
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
    Reqs/sec     24277.44    6486.15   43366.70
    Latency        2.06ms     1.92ms   162.23ms
    HTTP codes:
      1xx - 0, 2xx - 98256, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1744
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1744
    Throughput:     5.46MB/s
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
    Reqs/sec     73765.77    6077.63   94341.09
    Latency      675.52us   187.43us    14.39ms
    HTTP codes:
      1xx - 0, 2xx - 97821, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2179
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2179
    Throughput:    11.41MB/s
  ```


