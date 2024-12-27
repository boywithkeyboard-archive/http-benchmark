## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73773` | `6008` | `78150` |
| **87%** | [Hyper Express](#hyper-express) | `64156` | `4082` | `69015` |
| **32%** | [Node (Default)](#node-default) | `23931` | `6435` | `46632` |
| **30%** | [Fastify](#fastify) | `22387` | `5539` | `35580` |
| **26%** | [Hono](#hono) | `19398` | `4667` | `28671` |
| **24%** | [Koa](#koa) | `17891` | `5493` | `54953` |
| **11%** | [Carbon](#carbon) | `8267` | `1451` | `10514` |
| **9%** | [Express](#express) | `6313` | `910` | `8347` |


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
    Reqs/sec      8602.43    3630.18   48146.51
    Latency        5.80ms     4.20ms   365.14ms
    HTTP codes:
      1xx - 0, 2xx - 95256, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4744
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4744
    Throughput:     1.86MB/s
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
    Reqs/sec      6508.40     963.28    8655.32
    Latency        7.68ms     3.45ms   328.94ms
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
    Reqs/sec     22692.69    5553.16   35380.30
    Latency        2.20ms     1.96ms   178.02ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.15MB/s
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
    Reqs/sec     20121.31    5321.44   29658.66
    Latency        2.48ms     1.93ms   172.28ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.55MB/s
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
    Reqs/sec     63570.63    3332.12   76043.17
    Latency      784.20us    68.62us     3.56ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.03MB/s
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
    Reqs/sec     17605.99    5353.42   42439.28
    Latency        2.83ms     2.40ms   205.72ms
    HTTP codes:
      1xx - 0, 2xx - 95559, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4441
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4441
    Throughput:     3.81MB/s
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
    Reqs/sec     25848.58    7742.80   55973.59
    Latency        1.93ms     1.88ms   162.44ms
    HTTP codes:
      1xx - 0, 2xx - 97390, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2610
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2610
    Throughput:     5.76MB/s
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
    Reqs/sec     74556.51    6670.60   89700.91
    Latency      669.59us   241.98us    16.28ms
    HTTP codes:
      1xx - 0, 2xx - 97395, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2605
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2605
    Throughput:    11.47MB/s
  ```


