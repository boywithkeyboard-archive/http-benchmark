## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72832` | `5362` | `85699` |
| **82%** | [Hyper Express](#hyper-express) | `59843` | `4169` | `69036` |
| **29%** | [Fastify](#fastify) | `21124` | `5309` | `36509` |
| **29%** | [Hono](#hono) | `21015` | `6374` | `31452` |
| **28%** | [Node (Default)](#node-default) | `20607` | `5794` | `68047` |
| **23%** | [Koa](#koa) | `17026` | `8529` | `76516` |
| **10%** | [Carbon](#carbon) | `7434` | `1266` | `10207` |
| **8%** | [Express](#express) | `6104` | `1001` | `8276` |


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
    Reqs/sec      7784.95    4837.31   62557.87
    Latency        6.41ms     4.40ms   378.79ms
    HTTP codes:
      1xx - 0, 2xx - 93075, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6925
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6925
    Throughput:     1.65MB/s
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
    Reqs/sec      6153.52    1046.19    8217.76
    Latency        8.12ms     3.82ms   365.94ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.76MB/s
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
    Reqs/sec     20126.37    4809.48   38284.82
    Latency        2.48ms     2.08ms   186.67ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.56MB/s
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
    Reqs/sec     21848.18    7038.99   31409.38
    Latency        2.29ms     2.00ms   180.98ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.93MB/s
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
    Reqs/sec     60446.69    3750.56   67703.53
    Latency      824.90us    96.31us     4.63ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.59MB/s
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
    Reqs/sec     17806.87    8569.45   69955.82
    Latency        2.80ms     2.55ms   222.47ms
    HTTP codes:
      1xx - 0, 2xx - 92373, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7627
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7627
    Throughput:     3.72MB/s
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
    Reqs/sec     20287.36    5745.32   64668.15
    Latency        2.46ms     2.01ms   166.47ms
    HTTP codes:
      1xx - 0, 2xx - 96579, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3421
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3421
    Throughput:     4.49MB/s
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
    Reqs/sec     72761.56    4188.62   86230.62
    Latency      683.56us   210.39us    12.38ms
    HTTP codes:
      1xx - 0, 2xx - 96196, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3804
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3804
    Throughput:    11.08MB/s
  ```


