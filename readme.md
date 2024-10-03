## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74272` | `8271` | `92693` |
| **84%** | [Hyper Express](#hyper-express) | `62150` | `4014` | `71143` |
| **36%** | [Node (Default)](#node-default) | `26869` | `8434` | `54232` |
| **33%** | [Fastify](#fastify) | `24304` | `7520` | `36938` |
| **29%** | [Hono](#hono) | `21754` | `5851` | `30737` |
| **26%** | [Koa](#koa) | `19189` | `6398` | `48590` |
| **11%** | [Carbon](#carbon) | `8236` | `1337` | `10396` |
| **9%** | [Express](#express) | `6370` | `992` | `8385` |


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
    Reqs/sec      8840.05    3805.88   44715.40
    Latency        5.65ms     4.66ms   395.60ms
    HTTP codes:
      1xx - 0, 2xx - 94057, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5943
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5943
    Throughput:     1.89MB/s
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
    Reqs/sec      7048.31    5061.07   61526.77
    Latency        7.09ms     3.96ms   361.39ms
    HTTP codes:
      1xx - 0, 2xx - 90767, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9233
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9233
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
    Reqs/sec     24548.97    7278.25   37265.49
    Latency        2.04ms     1.97ms   177.89ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.57MB/s
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
    Reqs/sec     21857.24    6167.92   30503.67
    Latency        2.29ms     1.95ms   177.40ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.94MB/s
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
    Reqs/sec     63046.47    3278.07   67395.65
    Latency      790.49us   107.39us     4.43ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.96MB/s
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
    Reqs/sec     19470.44    7378.78   60092.81
    Latency        2.56ms     2.46ms   213.37ms
    HTTP codes:
      1xx - 0, 2xx - 93158, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6842
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6842
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
    Reqs/sec     26999.09    7718.85   55793.80
    Latency        1.85ms     1.89ms   163.96ms
    HTTP codes:
      1xx - 0, 2xx - 97444, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2556
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2556
    Throughput:     6.03MB/s
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
    Reqs/sec     74475.12    4778.46   84230.34
    Latency      669.51us   157.36us     6.43ms
    HTTP codes:
      1xx - 0, 2xx - 96925, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3075
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3075
    Throughput:    11.40MB/s
  ```


