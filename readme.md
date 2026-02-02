## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `71215` | `3450` | `77989` |
| **83%** | [Hyper Express](#hyper-express) | `59249` | `3421` | `62047` |
| **34%** | [Node (Default)](#node-default) | `23903` | `7281` | `76640` |
| **33%** | [Fastify](#fastify) | `23474` | `8000` | `38629` |
| **29%** | [Hono](#hono) | `20471` | `6168` | `30124` |
| **26%** | [Koa](#koa) | `18343` | `8392` | `69322` |
| **11%** | [Carbon](#carbon) | `8050` | `1499` | `10296` |
| **9%** | [Express](#express) | `6103` | `1058` | `8301` |


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
    Reqs/sec      8816.44    6640.72   80084.72
    Latency        5.66ms     4.40ms   382.40ms
    HTTP codes:
      1xx - 0, 2xx - 90076, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9924
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9924
    Throughput:     1.80MB/s
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
    Reqs/sec      6212.05    1050.32    8389.30
    Latency        8.04ms     3.72ms   355.74ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.78MB/s
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
    Reqs/sec     22729.67    7192.65   36434.25
    Latency        2.20ms     2.12ms   188.81ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.16MB/s
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
    Reqs/sec     20409.18    6047.11   30107.73
    Latency        2.45ms     2.07ms   184.39ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.61MB/s
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
    Reqs/sec     60377.90    3278.82   63920.34
    Latency      825.93us    77.74us     3.33ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.58MB/s
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
    Reqs/sec     18630.21    8903.58   79802.06
    Latency        2.68ms     2.48ms   215.76ms
    HTTP codes:
      1xx - 0, 2xx - 91823, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8177
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8177
    Throughput:     3.86MB/s
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
    Reqs/sec     23796.49    7357.79   63325.58
    Latency        2.10ms     2.00ms   170.10ms
    HTTP codes:
      1xx - 0, 2xx - 97360, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2640
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2640
    Throughput:     5.31MB/s
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
    Reqs/sec     73212.47    2873.30   82519.33
    Latency      680.04us   175.14us     7.56ms
    HTTP codes:
      1xx - 0, 2xx - 96679, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3321
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3321
    Throughput:    11.20MB/s
  ```


