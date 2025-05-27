## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74750` | `4726` | `81153` |
| **84%** | [Hyper Express](#hyper-express) | `62429` | `3518` | `70135` |
| **35%** | [Node (Default)](#node-default) | `26471` | `7187` | `48645` |
| **30%** | [Fastify](#fastify) | `22432` | `6717` | `36401` |
| **28%** | [Hono](#hono) | `21012` | `6119` | `30292` |
| **25%** | [Koa](#koa) | `18667` | `6508` | `51348` |
| **11%** | [Carbon](#carbon) | `8173` | `1424` | `10157` |
| **8%** | [Express](#express) | `6294` | `1015` | `8247` |


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
    Reqs/sec      8699.18    4938.78   59580.51
    Latency        5.74ms     4.36ms   378.74ms
    HTTP codes:
      1xx - 0, 2xx - 92085, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7915
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7915
    Throughput:     1.82MB/s
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
    Reqs/sec      6361.92     977.91    8491.59
    Latency        7.86ms     3.77ms   362.17ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.82MB/s
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
    Reqs/sec     23882.09    7538.07   37172.16
    Latency        2.09ms     2.03ms   180.95ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.42MB/s
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
    Reqs/sec     21039.09    6176.16   30503.56
    Latency        2.37ms     2.13ms   191.12ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.75MB/s
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
    Reqs/sec     61355.85    3186.30   63520.86
    Latency      812.56us    70.36us     3.52ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.72MB/s
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
    Reqs/sec     19333.52    7074.97   55958.79
    Latency        2.58ms     2.41ms   211.92ms
    HTTP codes:
      1xx - 0, 2xx - 93304, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6696
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6696
    Throughput:     4.08MB/s
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
    Reqs/sec     27430.78    8669.40   57459.14
    Latency        1.82ms     1.90ms   161.98ms
    HTTP codes:
      1xx - 0, 2xx - 96365, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3635
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3635
    Throughput:     6.05MB/s
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
    Reqs/sec     74045.09    4637.64   83490.17
    Latency      672.99us   171.20us     6.68ms
    HTTP codes:
      1xx - 0, 2xx - 96888, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3112
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3112
    Throughput:    11.35MB/s
  ```


