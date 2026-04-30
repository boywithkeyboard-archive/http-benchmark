## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `77721` | `5805` | `85277` |
| **91%** | [Hyper Express](#hyper-express) | `70909` | `2843` | `74274` |
| **44%** | [Node (Default)](#node-default) | `34424` | `9678` | `73633` |
| **44%** | [Fastify](#fastify) | `33917` | `10263` | `51996` |
| **41%** | [Koa](#koa) | `31858` | `14658` | `89522` |
| **40%** | [Hono](#hono) | `31155` | `9573` | `46287` |
| **12%** | [Carbon](#carbon) | `9163` | `2241` | `13340` |
| **9%** | [Express](#express) | `7367` | `1700` | `10671` |


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
    Reqs/sec     10265.41    8858.33   95174.08
    Latency        4.86ms     4.23ms   365.16ms
    HTTP codes:
      1xx - 0, 2xx - 87972, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12028
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12028
    Throughput:     2.05MB/s
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
    Reqs/sec      8103.13    7636.96   88385.42
    Latency        6.16ms     3.82ms   344.44ms
    HTTP codes:
      1xx - 0, 2xx - 88121, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11879
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11879
    Throughput:     2.05MB/s
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
    Reqs/sec     31841.14    8870.86   50302.57
    Latency        1.57ms     1.99ms   174.96ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.21MB/s
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
    Reqs/sec     30540.16    9246.48   47431.50
    Latency        1.64ms     2.03ms   176.79ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     6.88MB/s
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
    Reqs/sec     70245.85    3602.09   73910.24
    Latency      710.10us    73.30us     2.78ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.98MB/s
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
    Reqs/sec     28312.72   12981.37   86761.09
    Latency        1.76ms     2.28ms   197.27ms
    HTTP codes:
      1xx - 0, 2xx - 91103, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8897
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8897
    Throughput:     5.84MB/s
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
    Reqs/sec     33875.76    9968.38   81275.04
    Latency        1.47ms     1.82ms   153.49ms
    HTTP codes:
      1xx - 0, 2xx - 96116, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3884
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3884
    Throughput:     7.46MB/s
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
    Reqs/sec     81781.44    2993.20   86788.21
    Latency      609.07us   132.39us     7.25ms
    HTTP codes:
      1xx - 0, 2xx - 96831, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3169
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3169
    Throughput:    12.52MB/s
  ```


