## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `71978` | `3600` | `79833` |
| **84%** | [Hyper Express](#hyper-express) | `60559` | `3088` | `68299` |
| **33%** | [Node (Default)](#node-default) | `23812` | `7123` | `72625` |
| **33%** | [Fastify](#fastify) | `23728` | `7960` | `37406` |
| **29%** | [Hono](#hono) | `20937` | `6199` | `30187` |
| **26%** | [Koa](#koa) | `18780` | `9708` | `75833` |
| **11%** | [Carbon](#carbon) | `8229` | `1457` | `10416` |
| **9%** | [Express](#express) | `6468` | `1135` | `8384` |


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
    Reqs/sec      8667.67    5189.00   64404.64
    Latency        5.75ms     4.35ms   375.46ms
    HTTP codes:
      1xx - 0, 2xx - 92631, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7369
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7369
    Throughput:     1.83MB/s
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
    Reqs/sec      6502.63    1135.01    8360.60
    Latency        7.69ms     3.74ms   357.82ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.86MB/s
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
    Reqs/sec     24189.11    7665.52   37275.69
    Latency        2.07ms     2.11ms   187.09ms
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
    Reqs/sec     21612.41    6316.05   30681.79
    Latency        2.31ms     2.07ms   186.62ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.88MB/s
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
    Reqs/sec     60249.83    3825.62   67600.69
    Latency      827.69us    81.39us     2.94ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.56MB/s
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
    Reqs/sec     18802.05    8665.70   72440.45
    Latency        2.65ms     2.49ms   218.52ms
    HTTP codes:
      1xx - 0, 2xx - 91417, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8583
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8583
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
    Reqs/sec     23948.05    6315.92   61057.10
    Latency        2.09ms     1.96ms   168.15ms
    HTTP codes:
      1xx - 0, 2xx - 97440, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2560
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2560
    Throughput:     5.34MB/s
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
    Reqs/sec     71345.34    3197.57   78473.16
    Latency      698.11us   197.31us    11.81ms
    HTTP codes:
      1xx - 0, 2xx - 96281, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3719
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3719
    Throughput:    10.87MB/s
  ```


