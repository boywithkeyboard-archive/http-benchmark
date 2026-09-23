## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `178291` | `11022` | `201586` |
| **85%** | [Hyper Express](#hyper-express) | `151398` | `9805` | `158117` |
| **37%** | [Node (Default)](#node-default) | `65986` | `16441` | `146217` |
| **34%** | [Fastify](#fastify) | `61165` | `11442` | `70196` |
| **30%** | [Koa](#koa) | `52956` | `22337` | `159913` |
| **27%** | [Hono](#hono) | `47696` | `8175` | `58680` |
| **13%** | [Carbon](#carbon) | `22590` | `5220` | `31383` |
| **9%** | [Express](#express) | `15376` | `2835` | `21149` |


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
    Reqs/sec     22654.00   16568.46  140952.80
    Latency        2.19ms     2.92ms   233.39ms
    HTTP codes:
      1xx - 0, 2xx - 87568, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12432
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12432
    Throughput:     4.53MB/s
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
    Reqs/sec     16747.22   16014.97  141869.91
    Latency        2.98ms     2.38ms   215.69ms
    HTTP codes:
      1xx - 0, 2xx - 86019, 3xx - 0, 4xx - 0, 5xx - 0
      others - 13981
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 13981
    Throughput:     4.13MB/s
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
    Reqs/sec     71448.20   32919.34  165307.17
    Latency      696.39us     0.91ms    65.00ms
    HTTP codes:
      1xx - 0, 2xx - 73437, 3xx - 0, 4xx - 0, 5xx - 0
      others - 26563
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 26563
    Throughput:    11.92MB/s
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
    Reqs/sec     49985.21   20318.09  156464.22
    Latency        1.00ms     0.92ms    64.25ms
    HTTP codes:
      1xx - 0, 2xx - 88867, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11133
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11133
    Throughput:    10.04MB/s
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
    Reqs/sec    151981.61    9141.33  161462.81
    Latency      326.64us   153.31us     5.56ms
    HTTP codes:
      1xx - 0, 2xx - 90300, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9700
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9700
    Throughput:    19.50MB/s
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
    Reqs/sec     46418.33   19484.28  159597.48
    Latency        1.07ms     0.98ms    77.08ms
    HTTP codes:
      1xx - 0, 2xx - 89523, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10477
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10477
    Throughput:     9.40MB/s
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
    Reqs/sec     66738.87   17153.84  165631.70
    Latency      746.42us   647.90us    50.79ms
    HTTP codes:
      1xx - 0, 2xx - 94369, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5631
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5631
    Throughput:    14.43MB/s
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
    Reqs/sec    178609.04    8642.06  186840.37
    Latency      277.01us   137.49us     8.42ms
    HTTP codes:
      1xx - 0, 2xx - 95911, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4089
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4089
    Throughput:    27.09MB/s
  ```


