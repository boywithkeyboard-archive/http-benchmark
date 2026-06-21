## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `70940` | `3657` | `83087` |
| **85%** | [Hyper Express](#hyper-express) | `60071` | `3496` | `67969` |
| **30%** | [Hono](#hono) | `21035` | `6158` | `31702` |
| **29%** | [Fastify](#fastify) | `20857` | `5901` | `36875` |
| **29%** | [Node (Default)](#node-default) | `20475` | `5164` | `64432` |
| **27%** | [Koa](#koa) | `19192` | `8865` | `67365` |
| **10%** | [Carbon](#carbon) | `7366` | `1212` | `10607` |
| **9%** | [Express](#express) | `6273` | `1097` | `8330` |


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
    Reqs/sec      8239.79    6463.06   80102.18
    Latency        6.06ms     4.34ms   373.68ms
    HTTP codes:
      1xx - 0, 2xx - 89768, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10232
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10232
    Throughput:     1.68MB/s
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
    Reqs/sec      6388.50    1149.65    8462.49
    Latency        7.82ms     3.75ms   358.96ms
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
    Reqs/sec     20318.13    5095.57   37185.06
    Latency        2.46ms     2.10ms   188.02ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.61MB/s
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
    Reqs/sec     21695.52    6405.39   30062.65
    Latency        2.30ms     1.98ms   180.20ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.90MB/s
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
    Reqs/sec     58263.09    3563.70   65445.17
    Latency        0.86ms   104.53us     3.49ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.28MB/s
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
    Reqs/sec     18695.26    8370.02   65972.50
    Latency        2.67ms     2.38ms   210.57ms
    HTTP codes:
      1xx - 0, 2xx - 93042, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6958
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6958
    Throughput:     3.93MB/s
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
    Reqs/sec     20739.58    6198.51   71515.55
    Latency        2.41ms     1.97ms   169.40ms
    HTTP codes:
      1xx - 0, 2xx - 96068, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3932
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3932
    Throughput:     4.56MB/s
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
    Reqs/sec     71592.12    4501.10   83995.97
    Latency      695.72us   164.67us     6.55ms
    HTTP codes:
      1xx - 0, 2xx - 96996, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3004
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3004
    Throughput:    10.99MB/s
  ```


