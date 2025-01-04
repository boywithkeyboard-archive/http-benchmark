## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74571` | `6418` | `94625` |
| **87%** | [Hyper Express](#hyper-express) | `64552` | `4123` | `76908` |
| **35%** | [Node (Default)](#node-default) | `26377` | `7651` | `47518` |
| **33%** | [Fastify](#fastify) | `24528` | `7497` | `36085` |
| **27%** | [Hono](#hono) | `20466` | `5235` | `29638` |
| **24%** | [Koa](#koa) | `18098` | `6707` | `51581` |
| **11%** | [Carbon](#carbon) | `8377` | `1447` | `10670` |
| **9%** | [Express](#express) | `6533` | `1030` | `8632` |


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
    Reqs/sec      8814.60    4356.55   57566.79
    Latency        5.66ms     4.15ms   362.08ms
    HTTP codes:
      1xx - 0, 2xx - 94127, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5873
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5873
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
    Reqs/sec      7065.26    4949.74   61489.53
    Latency        7.02ms     3.85ms   357.28ms
    HTTP codes:
      1xx - 0, 2xx - 89927, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10073
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10068
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 5
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
    Reqs/sec     24175.35    6978.08   35338.89
    Latency        2.07ms     1.97ms   179.06ms
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
    Reqs/sec     21063.31    5885.95   30133.46
    Latency        2.37ms     1.92ms   175.83ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.76MB/s
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
    Reqs/sec     63741.30    4171.39   71645.83
    Latency      782.21us    74.72us     2.98ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.05MB/s
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
    Reqs/sec     17750.14    6215.77   56596.97
    Latency        2.81ms     2.37ms   207.04ms
    HTTP codes:
      1xx - 0, 2xx - 94637, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5363
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5363
    Throughput:     3.80MB/s
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
    Reqs/sec     25380.99    7440.56   57037.48
    Latency        1.97ms     1.89ms   160.91ms
    HTTP codes:
      1xx - 0, 2xx - 97062, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2938
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2938
    Throughput:     5.63MB/s
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
    Reqs/sec     75776.43    6893.99   89309.14
    Latency      656.46us   222.48us    11.96ms
    HTTP codes:
      1xx - 0, 2xx - 96806, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3194
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3194
    Throughput:    11.61MB/s
  ```


