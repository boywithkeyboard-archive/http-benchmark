## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74352` | `2427` | `77513` |
| **83%** | [Hyper Express](#hyper-express) | `61811` | `3937` | `65332` |
| **32%** | [Node (Default)](#node-default) | `24049` | `8032` | `70559` |
| **30%** | [Fastify](#fastify) | `22643` | `6122` | `35163` |
| **28%** | [Hono](#hono) | `20708` | `5924` | `29878` |
| **26%** | [Koa](#koa) | `19346` | `9364` | `78658` |
| **11%** | [Carbon](#carbon) | `8105` | `1398` | `10288` |
| **9%** | [Express](#express) | `6352` | `1077` | `8309` |


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
    Reqs/sec      8864.77    6626.07   70670.31
    Latency        5.63ms     4.30ms   369.18ms
    HTTP codes:
      1xx - 0, 2xx - 89949, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10051
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10051
    Throughput:     1.81MB/s
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
    Reqs/sec      6127.77     981.46    8204.30
    Latency        8.16ms     3.78ms   361.65ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.75MB/s
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
    Reqs/sec     24291.26    7311.49   35285.07
    Latency        2.06ms     1.94ms   175.76ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.51MB/s
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
    Reqs/sec     20609.38    5956.21   30085.18
    Latency        2.42ms     2.03ms   182.44ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.66MB/s
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
    Reqs/sec     63445.46    3510.31   67675.29
    Latency      786.08us    72.09us     3.66ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.01MB/s
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
    Reqs/sec     19497.61    9693.46   80982.85
    Latency        2.55ms     2.57ms   223.50ms
    HTTP codes:
      1xx - 0, 2xx - 90371, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9629
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9629
    Throughput:     4.00MB/s
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
    Reqs/sec     25257.08    7786.96   60635.79
    Latency        1.98ms     1.93ms   166.28ms
    HTTP codes:
      1xx - 0, 2xx - 97495, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2505
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2505
    Throughput:     5.64MB/s
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
    Reqs/sec     75155.52    2646.50   85757.80
    Latency      661.94us   217.60us    11.76ms
    HTTP codes:
      1xx - 0, 2xx - 96298, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3702
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3702
    Throughput:    11.46MB/s
  ```


