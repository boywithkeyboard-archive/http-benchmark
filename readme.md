## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74784` | `2591` | `79876` |
| **83%** | [Hyper Express](#hyper-express) | `62013` | `3884` | `65573` |
| **32%** | [Node (Default)](#node-default) | `24145` | `7165` | `74748` |
| **30%** | [Fastify](#fastify) | `22498` | `6440` | `35429` |
| **26%** | [Hono](#hono) | `19763` | `5104` | `29594` |
| **24%** | [Koa](#koa) | `17742` | `8429` | `80985` |
| **11%** | [Carbon](#carbon) | `7856` | `1339` | `10330` |
| **8%** | [Express](#express) | `6013` | `955` | `8129` |


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
    Reqs/sec      8843.94    6939.37   79685.36
    Latency        5.64ms     4.37ms   374.92ms
    HTTP codes:
      1xx - 0, 2xx - 89444, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10556
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10556
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
    Reqs/sec      6208.19     996.53    8295.58
    Latency        8.05ms     3.73ms   358.97ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.78MB/s
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
    Reqs/sec     22140.43    5964.44   35921.77
    Latency        2.26ms     2.11ms   191.28ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.02MB/s
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
    Reqs/sec     19519.61    4847.15   29333.52
    Latency        2.56ms     1.97ms   180.71ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.41MB/s
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
    Reqs/sec     61928.82    5318.86   75059.64
    Latency      805.31us    93.26us     5.46ms
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
    Reqs/sec     18140.06    8521.97   75838.00
    Latency        2.75ms     2.32ms   206.90ms
    HTTP codes:
      1xx - 0, 2xx - 91422, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8578
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8578
    Throughput:     3.76MB/s
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
    Reqs/sec     23600.55    7371.36   77950.65
    Latency        2.11ms     1.96ms   169.71ms
    HTTP codes:
      1xx - 0, 2xx - 96629, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3371
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3371
    Throughput:     5.22MB/s
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
    Reqs/sec     74584.67    3088.07   82634.96
    Latency      668.20us   185.06us    11.51ms
    HTTP codes:
      1xx - 0, 2xx - 96374, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3626
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3626
    Throughput:    11.36MB/s
  ```


