## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73292` | `2010` | `80108` |
| **83%** | [Hyper Express](#hyper-express) | `60824` | `3362` | `64651` |
| **34%** | [Node (Default)](#node-default) | `24561` | `7438` | `61940` |
| **31%** | [Fastify](#fastify) | `22605` | `6752` | `35572` |
| **27%** | [Hono](#hono) | `19971` | `6136` | `29851` |
| **25%** | [Koa](#koa) | `18331` | `9210` | `80400` |
| **11%** | [Carbon](#carbon) | `8173` | `1458` | `10296` |
| **9%** | [Express](#express) | `6308` | `1069` | `8163` |


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
    Reqs/sec      8727.06    5400.39   69735.04
    Latency        5.71ms     4.37ms   384.98ms
    HTTP codes:
      1xx - 0, 2xx - 92453, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7547
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7547
    Throughput:     1.84MB/s
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
    Reqs/sec      6322.77    1065.69    8228.86
    Latency        7.90ms     3.84ms   366.13ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.81MB/s
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
    Reqs/sec     23558.31    7299.59   35163.11
    Latency        2.12ms     2.14ms   192.35ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.35MB/s
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
    Reqs/sec     20783.99    5946.26   29120.45
    Latency        2.40ms     2.04ms   181.37ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.70MB/s
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
    Reqs/sec     61176.20    3069.60   64311.81
    Latency      814.76us    71.42us     3.00ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.69MB/s
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
    Reqs/sec     18246.15    9463.28   77189.01
    Latency        2.73ms     2.38ms   210.32ms
    HTTP codes:
      1xx - 0, 2xx - 90071, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9929
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9929
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
    Reqs/sec     25097.74    9431.45   82400.76
    Latency        1.99ms     1.93ms   169.92ms
    HTTP codes:
      1xx - 0, 2xx - 94899, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5101
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5101
    Throughput:     5.46MB/s
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
    Reqs/sec     73267.13    2129.82   80725.40
    Latency      679.36us   154.19us     7.59ms
    HTTP codes:
      1xx - 0, 2xx - 96560, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3440
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3440
    Throughput:    11.20MB/s
  ```


