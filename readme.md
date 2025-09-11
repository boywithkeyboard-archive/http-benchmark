## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75189` | `3574` | `86641` |
| **83%** | [Hyper Express](#hyper-express) | `62577` | `3549` | `67744` |
| **34%** | [Node (Default)](#node-default) | `25324` | `8218` | `77604` |
| **33%** | [Fastify](#fastify) | `24750` | `7727` | `35805` |
| **27%** | [Hono](#hono) | `20365` | `6183` | `30557` |
| **25%** | [Koa](#koa) | `18928` | `8063` | `68470` |
| **11%** | [Carbon](#carbon) | `7996` | `1373` | `10247` |
| **8%** | [Express](#express) | `6311` | `1019` | `8367` |


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
    Reqs/sec      9109.07    7554.36   79480.21
    Latency        5.48ms     4.31ms   374.80ms
    HTTP codes:
      1xx - 0, 2xx - 88000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12000
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12000
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
    Reqs/sec      6843.42    5324.82   67098.00
    Latency        7.28ms     4.04ms   370.17ms
    HTTP codes:
      1xx - 0, 2xx - 91113, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8887
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8887
    Throughput:     1.79MB/s
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
    Reqs/sec     24170.82    7598.39   35867.41
    Latency        2.07ms     2.09ms   185.10ms
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
    Reqs/sec     21706.10    6258.69   30231.27
    Latency        2.30ms     2.12ms   187.56ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.90MB/s
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
    Reqs/sec     63374.99    3508.86   66318.72
    Latency      786.85us    71.33us     2.52ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.00MB/s
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
    Reqs/sec     19336.34    8308.05   71234.50
    Latency        2.58ms     2.41ms   210.32ms
    HTTP codes:
      1xx - 0, 2xx - 92094, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7906
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7906
    Throughput:     4.03MB/s
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
    Reqs/sec     24692.47    7670.19   61659.29
    Latency        2.02ms     1.92ms   165.85ms
    HTTP codes:
      1xx - 0, 2xx - 97556, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2444
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2444
    Throughput:     5.51MB/s
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
    Reqs/sec     75113.38    3154.75   89819.23
    Latency      662.39us   195.50us    10.82ms
    HTTP codes:
      1xx - 0, 2xx - 96504, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3496
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3496
    Throughput:    11.47MB/s
  ```


