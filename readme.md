## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75268` | `3596` | `86012` |
| **84%** | [Hyper Express](#hyper-express) | `62960` | `3721` | `66129` |
| **33%** | [Node (Default)](#node-default) | `24868` | `7630` | `63643` |
| **32%** | [Fastify](#fastify) | `23945` | `7217` | `35404` |
| **27%** | [Hono](#hono) | `20311` | `5621` | `29313` |
| **25%** | [Koa](#koa) | `19191` | `8898` | `74163` |
| **11%** | [Carbon](#carbon) | `8324` | `1546` | `10362` |
| **8%** | [Express](#express) | `6202` | `1034` | `8282` |


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
    Reqs/sec      8520.38    5898.33   82755.43
    Latency        5.86ms     4.36ms   376.51ms
    HTTP codes:
      1xx - 0, 2xx - 91688, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8312
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8312
    Throughput:     1.77MB/s
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
    Reqs/sec      6313.10    1075.81    8229.04
    Latency        7.92ms     3.64ms   350.48ms
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
    Reqs/sec     24084.58    7378.77   35090.65
    Latency        2.08ms     2.01ms   181.31ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.46MB/s
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
    Reqs/sec     20667.01    5698.19   28914.21
    Latency        2.42ms     2.04ms   183.67ms
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
    Reqs/sec     62856.00    3348.84   66073.59
    Latency      793.49us    65.74us     2.60ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.93MB/s
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
    Reqs/sec     19373.14   10608.33   84035.72
    Latency        2.58ms     2.47ms   215.19ms
    HTTP codes:
      1xx - 0, 2xx - 88141, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11859
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11859
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
    Reqs/sec     24635.28    7563.94   63815.29
    Latency        2.03ms     2.01ms   172.56ms
    HTTP codes:
      1xx - 0, 2xx - 97435, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2565
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2565
    Throughput:     5.50MB/s
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
    Reqs/sec     75419.57    3125.71   85269.27
    Latency      660.15us   154.07us     7.25ms
    HTTP codes:
      1xx - 0, 2xx - 96915, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3085
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3085
    Throughput:    11.57MB/s
  ```


