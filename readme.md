## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73797` | `6276` | `81214` |
| **84%** | [Hyper Express](#hyper-express) | `61874` | `3362` | `65126` |
| **36%** | [Node (Default)](#node-default) | `26302` | `8143` | `49477` |
| **32%** | [Fastify](#fastify) | `23607` | `7042` | `35290` |
| **27%** | [Hono](#hono) | `20126` | `5231` | `28805` |
| **25%** | [Koa](#koa) | `18569` | `6632` | `57875` |
| **11%** | [Carbon](#carbon) | `8113` | `1337` | `10520` |
| **9%** | [Express](#express) | `6442` | `1010` | `8522` |


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
    Reqs/sec      8897.40    4253.13   52302.07
    Latency        5.61ms     4.16ms   361.65ms
    HTTP codes:
      1xx - 0, 2xx - 93809, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6191
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6191
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
    Reqs/sec      6486.26    1035.22    8580.03
    Latency        7.71ms     3.53ms   338.11ms
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
    Reqs/sec     23423.70    6859.87   35395.84
    Latency        2.13ms     1.97ms   178.89ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.32MB/s
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
    Reqs/sec     20550.09    5727.02   29136.82
    Latency        2.43ms     1.98ms   176.87ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.64MB/s
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
    Reqs/sec     62482.16    2560.82   68089.46
    Latency      798.02us    66.15us     5.13ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.87MB/s
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
    Reqs/sec     17644.00    6058.71   59482.36
    Latency        2.82ms     2.36ms   204.96ms
    HTTP codes:
      1xx - 0, 2xx - 94293, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5707
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5707
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
    Reqs/sec     25987.01    7532.66   44391.90
    Latency        1.92ms     1.83ms   158.64ms
    HTTP codes:
      1xx - 0, 2xx - 98093, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1907
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1907
    Throughput:     5.84MB/s
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
    Reqs/sec     73697.46    6710.03   88455.45
    Latency      674.59us   259.86us    13.69ms
    HTTP codes:
      1xx - 0, 2xx - 96572, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3428
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3428
    Throughput:    11.26MB/s
  ```


