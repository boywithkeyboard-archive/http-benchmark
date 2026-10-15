## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `118258` | `4758` | `124726` |
| **94%** | [Hyper Express](#hyper-express) | `110840` | `5889` | `114594` |
| **59%** | [Node (Default)](#node-default) | `69726` | `15956` | `110172` |
| **56%** | [Fastify](#fastify) | `66682` | `16192` | `84985` |
| **50%** | [Koa](#koa) | `58862` | `17475` | `98222` |
| **47%** | [Hono](#hono) | `55535` | `16296` | `77683` |
| **21%** | [Carbon](#carbon) | `25273` | `7897` | `37412` |
| **15%** | [Express](#express) | `17862` | `4088` | `24296` |


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
    Reqs/sec     26323.17   11595.72  106600.06
    Latency        1.89ms     3.01ms   251.40ms
    HTTP codes:
      1xx - 0, 2xx - 93730, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6270
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6270
    Throughput:     5.61MB/s
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
    Reqs/sec     18454.19   11293.94  117504.76
    Latency        2.70ms     2.21ms   199.56ms
    HTTP codes:
      1xx - 0, 2xx - 90703, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9297
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9297
    Throughput:     4.80MB/s
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
    Reqs/sec     69044.01   19988.33  116004.05
    Latency      720.08us     1.04ms    81.82ms
    HTTP codes:
      1xx - 0, 2xx - 83194, 3xx - 0, 4xx - 0, 5xx - 0
      others - 16806
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 16806
    Throughput:    13.07MB/s
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
    Reqs/sec     63265.17   17460.77   95866.05
    Latency      787.06us     1.15ms    83.53ms
    HTTP codes:
      1xx - 0, 2xx - 94616, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5384
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5384
    Throughput:    13.54MB/s
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
    Reqs/sec    111500.91    5015.72  115280.34
    Latency      445.97us   181.12us     8.81ms
    HTTP codes:
      1xx - 0, 2xx - 94254, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5746
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5746
    Throughput:    14.93MB/s
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
    Reqs/sec     60345.61   18751.03  126430.64
    Latency      826.16us     1.25ms    94.16ms
    HTTP codes:
      1xx - 0, 2xx - 91537, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8463
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8463
    Throughput:    12.49MB/s
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
    Reqs/sec     70197.41   18987.17  103399.90
    Latency      711.27us     1.10ms    90.58ms
    HTTP codes:
      1xx - 0, 2xx - 96546, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3454
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3454
    Throughput:    15.49MB/s
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
    Reqs/sec    122873.02    3519.58  128416.38
    Latency      405.54us   112.85us     6.92ms
    HTTP codes:
      1xx - 0, 2xx - 97573, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2427
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2427
    Throughput:    18.97MB/s
  ```


