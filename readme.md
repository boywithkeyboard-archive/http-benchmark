## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74806` | `2721` | `83788` |
| **85%** | [Hyper Express](#hyper-express) | `63292` | `3903` | `66459` |
| **36%** | [Node (Default)](#node-default) | `26767` | `8444` | `80328` |
| **32%** | [Fastify](#fastify) | `24043` | `7560` | `35978` |
| **29%** | [Hono](#hono) | `21670` | `6330` | `30960` |
| **25%** | [Koa](#koa) | `19018` | `8669` | `81107` |
| **11%** | [Carbon](#carbon) | `8440` | `1451` | `10559` |
| **9%** | [Express](#express) | `6422` | `1031` | `8368` |


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
    Reqs/sec      9279.92    6715.66   78029.00
    Latency        5.37ms     4.20ms   362.40ms
    HTTP codes:
      1xx - 0, 2xx - 90199, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9801
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9801
    Throughput:     1.90MB/s
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
    Reqs/sec      7192.46    6347.30   76284.36
    Latency        6.93ms     3.87ms   355.64ms
    HTTP codes:
      1xx - 0, 2xx - 89131, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10869
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10869
    Throughput:     1.84MB/s
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
    Reqs/sec     24160.57    7126.05   36028.88
    Latency        2.07ms     2.06ms   182.76ms
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
    Reqs/sec     20596.06    5500.20   30355.83
    Latency        2.43ms     1.91ms   174.47ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.65MB/s
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
    Reqs/sec     62491.75    3064.56   69229.43
    Latency      797.98us    68.78us     3.77ms
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
    Reqs/sec     19924.59    7966.92   66298.67
    Latency        2.50ms     2.30ms   202.52ms
    HTTP codes:
      1xx - 0, 2xx - 93411, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6589
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6589
    Throughput:     4.21MB/s
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
    Reqs/sec     25978.27    7715.91   65104.94
    Latency        1.92ms     1.83ms   158.84ms
    HTTP codes:
      1xx - 0, 2xx - 97212, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2788
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2788
    Throughput:     5.78MB/s
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
    Reqs/sec     74587.57    2100.76   81041.87
    Latency      667.64us   161.75us     9.43ms
    HTTP codes:
      1xx - 0, 2xx - 96363, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3637
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3637
    Throughput:    11.38MB/s
  ```


