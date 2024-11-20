## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73697` | `7473` | `85223` |
| **86%** | [Hyper Express](#hyper-express) | `63425` | `3490` | `66934` |
| **35%** | [Node (Default)](#node-default) | `26036` | `7620` | `63105` |
| **32%** | [Fastify](#fastify) | `23673` | `6577` | `35200` |
| **28%** | [Hono](#hono) | `21001` | `5457` | `29124` |
| **23%** | [Koa](#koa) | `17143` | `5357` | `48391` |
| **11%** | [Carbon](#carbon) | `8278` | `1358` | `10598` |
| **9%** | [Express](#express) | `6768` | `1159` | `9611` |


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
    Reqs/sec      8986.38    4346.47   55948.84
    Latency        5.55ms     4.18ms   361.83ms
    HTTP codes:
      1xx - 0, 2xx - 93800, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6200
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6200
    Throughput:     1.91MB/s
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
    Reqs/sec      6695.10    1071.35    8698.34
    Latency        7.47ms     3.51ms   335.32ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.91MB/s
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
    Reqs/sec     25055.56    7987.98   36742.77
    Latency        1.99ms     1.97ms   177.37ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.69MB/s
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
    Reqs/sec     20882.57    5692.60   29349.86
    Latency        2.39ms     1.92ms   171.36ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.72MB/s
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
    Reqs/sec     63815.26    3744.35   79627.29
    Latency      780.94us    68.88us     4.17ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.07MB/s
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
    Reqs/sec     19151.76    6236.97   47441.23
    Latency        2.61ms     2.32ms   204.72ms
    HTTP codes:
      1xx - 0, 2xx - 94673, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5327
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5327
    Throughput:     4.10MB/s
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
    Reqs/sec     24882.23    6667.64   42543.35
    Latency        2.01ms     1.85ms   162.19ms
    HTTP codes:
      1xx - 0, 2xx - 98279, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1721
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1721
    Throughput:     5.60MB/s
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
    Reqs/sec     74597.60    6355.99   85582.13
    Latency      668.86us   206.43us     9.71ms
    HTTP codes:
      1xx - 0, 2xx - 97088, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2912
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2912
    Throughput:    11.45MB/s
  ```


