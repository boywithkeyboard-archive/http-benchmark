## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75738` | `3022` | `82369` |
| **82%** | [Hyper Express](#hyper-express) | `62172` | `3776` | `65672` |
| **34%** | [Node (Default)](#node-default) | `25717` | `8426` | `68413` |
| **31%** | [Fastify](#fastify) | `23103` | `6896` | `33693` |
| **27%** | [Hono](#hono) | `20332` | `5663` | `28525` |
| **25%** | [Koa](#koa) | `18727` | `7890` | `62435` |
| **11%** | [Carbon](#carbon) | `8212` | `1428` | `10424` |
| **8%** | [Express](#express) | `6256` | `1006` | `8358` |


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
    Reqs/sec      9149.74    6966.50   77688.54
    Latency        5.45ms     4.25ms   365.92ms
    HTTP codes:
      1xx - 0, 2xx - 89563, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10437
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10437
    Throughput:     1.86MB/s
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
    Reqs/sec      7260.89    6444.91   69688.32
    Latency        6.88ms     3.86ms   354.17ms
    HTTP codes:
      1xx - 0, 2xx - 88831, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11169
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11169
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
    Reqs/sec     23946.93    6831.74   35830.70
    Latency        2.09ms     1.92ms   171.88ms
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
    Reqs/sec     21279.33    6100.43   29422.82
    Latency        2.35ms     2.02ms   181.98ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.81MB/s
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
    Reqs/sec     63414.34    3793.46   66759.56
    Latency      786.26us    71.61us     3.01ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.01MB/s
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
    Reqs/sec     18666.53    7863.42   66800.62
    Latency        2.67ms     2.36ms   209.34ms
    HTTP codes:
      1xx - 0, 2xx - 93354, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6646
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6646
    Throughput:     3.94MB/s
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
    Reqs/sec     26177.02    8694.98   70468.60
    Latency        1.91ms     1.91ms   168.95ms
    HTTP codes:
      1xx - 0, 2xx - 96649, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3351
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3351
    Throughput:     5.79MB/s
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
    Reqs/sec     75985.82    3032.15   84219.42
    Latency      655.26us   172.96us    10.12ms
    HTTP codes:
      1xx - 0, 2xx - 96574, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3426
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3426
    Throughput:    11.61MB/s
  ```


