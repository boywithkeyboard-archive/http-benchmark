## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74797` | `6253` | `86110` |
| **83%** | [Hyper Express](#hyper-express) | `62339` | `3512` | `69379` |
| **34%** | [Node (Default)](#node-default) | `25503` | `7099` | `45229` |
| **32%** | [Fastify](#fastify) | `24102` | `6938` | `35503` |
| **28%** | [Hono](#hono) | `21108` | `5843` | `29267` |
| **24%** | [Koa](#koa) | `17724` | `5429` | `38770` |
| **11%** | [Carbon](#carbon) | `8192` | `1376` | `10474` |
| **9%** | [Express](#express) | `6511` | `1039` | `8489` |


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
    Reqs/sec      8732.23    3816.91   57333.07
    Latency        5.72ms     4.24ms   369.12ms
    HTTP codes:
      1xx - 0, 2xx - 94838, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5162
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5162
    Throughput:     1.88MB/s
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
    Reqs/sec      6478.00    1006.47    8511.13
    Latency        7.72ms     3.56ms   342.10ms
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
    Reqs/sec     25338.32    7660.60   35619.26
    Latency        1.97ms     2.02ms   184.71ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.75MB/s
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
    Reqs/sec     20520.68    5526.91   29273.63
    Latency        2.43ms     2.05ms   184.92ms
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
    Reqs/sec     62580.53    3897.48   72712.05
    Latency      798.15us    69.45us     3.40ms
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
    Reqs/sec     17037.56    5506.05   43984.00
    Latency        2.91ms     2.43ms   209.21ms
    HTTP codes:
      1xx - 0, 2xx - 94975, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5025
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5025
    Throughput:     3.67MB/s
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
    Reqs/sec     25768.83    7199.73   40358.62
    Latency        1.94ms     1.90ms   164.95ms
    HTTP codes:
      1xx - 0, 2xx - 98305, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1695
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1695
    Throughput:     5.80MB/s
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
    Reqs/sec     74791.23    6425.12   88851.75
    Latency      666.71us   177.02us     9.07ms
    HTTP codes:
      1xx - 0, 2xx - 97882, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2118
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2118
    Throughput:    11.58MB/s
  ```


