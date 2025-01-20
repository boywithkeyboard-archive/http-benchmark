## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74206` | `4804` | `83961` |
| **83%** | [Hyper Express](#hyper-express) | `61788` | `4236` | `71847` |
| **34%** | [Node (Default)](#node-default) | `25425` | `7067` | `55575` |
| **32%** | [Fastify](#fastify) | `23597` | `7102` | `36225` |
| **27%** | [Hono](#hono) | `20035` | `5483` | `29571` |
| **25%** | [Koa](#koa) | `18286` | `6960` | `55222` |
| **11%** | [Carbon](#carbon) | `8222` | `1412` | `10381` |
| **8%** | [Express](#express) | `6234` | `919` | `8246` |


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
    Reqs/sec      8975.64    5049.87   61236.13
    Latency        5.56ms     4.27ms   369.58ms
    HTTP codes:
      1xx - 0, 2xx - 92744, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7256
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7256
    Throughput:     1.89MB/s
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
    Reqs/sec      6620.95    1119.45    8461.03
    Latency        7.55ms     3.49ms   335.26ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.89MB/s
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
    Reqs/sec     22900.77    6722.40   35552.88
    Latency        2.18ms     2.01ms   180.93ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.20MB/s
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
    Reqs/sec     20199.27    5383.71   30299.93
    Latency        2.47ms     2.05ms   180.35ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.56MB/s
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
    Reqs/sec     63601.78    4167.53   70656.33
    Latency      783.92us    79.01us     4.43ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.03MB/s
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
    Reqs/sec     18316.89    6863.94   56136.37
    Latency        2.72ms     2.45ms   212.48ms
    HTTP codes:
      1xx - 0, 2xx - 93830, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6170
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6170
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
    Reqs/sec     25026.06    7045.96   57042.90
    Latency        1.99ms     1.85ms   156.41ms
    HTTP codes:
      1xx - 0, 2xx - 97415, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2585
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2585
    Throughput:     5.58MB/s
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
    Reqs/sec     75373.90    5551.33   85133.01
    Latency      662.56us   177.96us     7.56ms
    HTTP codes:
      1xx - 0, 2xx - 97666, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2334
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2334
    Throughput:    11.63MB/s
  ```


