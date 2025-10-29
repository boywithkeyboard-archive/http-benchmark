## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74697` | `2917` | `82608` |
| **85%** | [Hyper Express](#hyper-express) | `63615` | `2936` | `67818` |
| **33%** | [Node (Default)](#node-default) | `24620` | `7657` | `62930` |
| **32%** | [Fastify](#fastify) | `24247` | `7647` | `35563` |
| **27%** | [Hono](#hono) | `20476` | `5937` | `29321` |
| **25%** | [Koa](#koa) | `18727` | `10280` | `76899` |
| **11%** | [Carbon](#carbon) | `8418` | `1539` | `10606` |
| **9%** | [Express](#express) | `6352` | `1088` | `8341` |


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
    Reqs/sec      9051.34    7584.14   78361.20
    Latency        5.51ms     4.42ms   381.25ms
    HTTP codes:
      1xx - 0, 2xx - 88130, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11870
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11870
    Throughput:     1.81MB/s
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
    Reqs/sec      7096.24    6524.75   74107.71
    Latency        7.04ms     3.98ms   363.14ms
    HTTP codes:
      1xx - 0, 2xx - 88495, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11505
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11505
    Throughput:     1.80MB/s
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
    Reqs/sec     24160.50    7294.18   35164.13
    Latency        2.07ms     2.02ms   182.34ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.48MB/s
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
    Reqs/sec     20358.44    5651.61   29592.82
    Latency        2.45ms     2.09ms   186.25ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.60MB/s
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
    Reqs/sec     62197.59    3765.09   65281.05
    Latency      801.98us    77.52us     3.14ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.83MB/s
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
    Reqs/sec     19078.33    9610.42   78993.35
    Latency        2.61ms     2.51ms   220.55ms
    HTTP codes:
      1xx - 0, 2xx - 90208, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9792
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9792
    Throughput:     3.89MB/s
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
    Reqs/sec     25165.34    8109.47   77225.95
    Latency        1.98ms     2.00ms   173.07ms
    HTTP codes:
      1xx - 0, 2xx - 96609, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3391
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3391
    Throughput:     5.57MB/s
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
    Reqs/sec     75318.24    3106.18   84107.04
    Latency      659.89us   155.94us     8.66ms
    HTTP codes:
      1xx - 0, 2xx - 96573, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3427
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3427
    Throughput:    11.51MB/s
  ```


