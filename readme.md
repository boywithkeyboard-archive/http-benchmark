## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74669` | `5408` | `88788` |
| **83%** | [Hyper Express](#hyper-express) | `61992` | `3562` | `65287` |
| **36%** | [Node (Default)](#node-default) | `26622` | `8012` | `60431` |
| **34%** | [Fastify](#fastify) | `25317` | `7921` | `36885` |
| **28%** | [Hono](#hono) | `21053` | `5796` | `29816` |
| **25%** | [Koa](#koa) | `18807` | `6445` | `50258` |
| **11%** | [Carbon](#carbon) | `8232` | `1387` | `10390` |
| **9%** | [Express](#express) | `6453` | `1038` | `8307` |


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
    Reqs/sec      8829.18    4493.80   56652.44
    Latency        5.64ms     4.48ms   382.98ms
    HTTP codes:
      1xx - 0, 2xx - 92860, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7140
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7140
    Throughput:     1.87MB/s
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
    Reqs/sec      7097.89    3989.41   45967.01
    Latency        7.03ms     3.94ms   359.56ms
    HTTP codes:
      1xx - 0, 2xx - 92896, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7104
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7104
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
    Reqs/sec     23796.70    7381.02   36195.32
    Latency        2.10ms     1.96ms   175.85ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.39MB/s
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
    Reqs/sec     20633.49    5786.43   29843.92
    Latency        2.42ms     1.96ms   177.71ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.67MB/s
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
    Reqs/sec     62123.82    3672.16   65389.70
    Latency      802.73us    66.86us     2.88ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.82MB/s
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
    Reqs/sec     18575.90    6087.86   51397.86
    Latency        2.69ms     2.45ms   212.13ms
    HTTP codes:
      1xx - 0, 2xx - 94679, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5321
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5321
    Throughput:     3.98MB/s
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
    Reqs/sec     26696.20    7567.51   44708.09
    Latency        1.87ms     1.94ms   165.94ms
    HTTP codes:
      1xx - 0, 2xx - 98239, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1761
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1761
    Throughput:     6.00MB/s
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
    Reqs/sec     73944.35    4760.78   82093.98
    Latency      673.09us   166.80us     6.92ms
    HTTP codes:
      1xx - 0, 2xx - 96518, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3482
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3482
    Throughput:    11.30MB/s
  ```


