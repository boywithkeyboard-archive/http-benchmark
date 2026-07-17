## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `140368` | `6053` | `148964` |
| **85%** | [Hyper Express](#hyper-express) | `119185` | `6675` | `126803` |
| **35%** | [Node (Default)](#node-default) | `49463` | `12206` | `109762` |
| **33%** | [Fastify](#fastify) | `46438` | `9951` | `76247` |
| **28%** | [Hono](#hono) | `39895` | `9568` | `64279` |
| **27%** | [Koa](#koa) | `38019` | `16429` | `130707` |
| **12%** | [Carbon](#carbon) | `17378` | `4834` | `26725` |
| **8%** | [Express](#express) | `11873` | `2230` | `17478` |


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
    Reqs/sec     19011.81   16348.30  128347.07
    Latency        2.62ms     3.56ms   294.50ms
    HTTP codes:
      1xx - 0, 2xx - 84047, 3xx - 0, 4xx - 0, 5xx - 0
      others - 15953
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 15953
    Throughput:     3.63MB/s
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
    Reqs/sec     13671.01   13806.97  130383.72
    Latency        3.65ms     2.80ms   247.66ms
    HTTP codes:
      1xx - 0, 2xx - 84679, 3xx - 0, 4xx - 0, 5xx - 0
      others - 15321
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 15321
    Throughput:     3.32MB/s
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
    Reqs/sec     53103.07   24982.85  128376.75
    Latency        0.94ms     1.24ms    87.08ms
    HTTP codes:
      1xx - 0, 2xx - 73338, 3xx - 0, 4xx - 0, 5xx - 0
      others - 26662
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 26662
    Throughput:     8.86MB/s
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
    Reqs/sec     43113.98   16838.58  127547.00
    Latency        1.16ms     1.49ms   110.66ms
    HTTP codes:
      1xx - 0, 2xx - 89388, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10612
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10612
    Throughput:     8.72MB/s
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
    Reqs/sec    119011.86    6243.64  129863.05
    Latency      417.98us   188.15us     7.84ms
    HTTP codes:
      1xx - 0, 2xx - 90973, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9027
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9027
    Throughput:    15.36MB/s
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
    Reqs/sec     38890.23   18682.85  154864.64
    Latency        1.28ms     1.32ms   107.91ms
    HTTP codes:
      1xx - 0, 2xx - 87425, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12575
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12575
    Throughput:     7.72MB/s
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
    Reqs/sec     48975.78   12508.45  120186.23
    Latency        1.02ms     1.08ms    88.17ms
    HTTP codes:
      1xx - 0, 2xx - 95562, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4438
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4438
    Throughput:    10.70MB/s
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
    Reqs/sec    139596.07    8306.20  152078.39
    Latency      355.70us   125.08us     5.95ms
    HTTP codes:
      1xx - 0, 2xx - 94552, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5448
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5448
    Throughput:    20.89MB/s
  ```


