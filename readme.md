## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75092` | `2846` | `82021` |
| **84%** | [Hyper Express](#hyper-express) | `63202` | `3799` | `65936` |
| **35%** | [Node (Default)](#node-default) | `26251` | `8175` | `64152` |
| **33%** | [Fastify](#fastify) | `24632` | `7409` | `36278` |
| **27%** | [Hono](#hono) | `20438` | `5716` | `29501` |
| **25%** | [Koa](#koa) | `18521` | `8524` | `71417` |
| **11%** | [Carbon](#carbon) | `8113` | `1370` | `10351` |
| **9%** | [Express](#express) | `6561` | `1125` | `8489` |


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
    Reqs/sec      8874.58    5784.89   65696.22
    Latency        5.62ms     4.25ms   367.10ms
    HTTP codes:
      1xx - 0, 2xx - 91754, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8246
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8246
    Throughput:     1.85MB/s
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
    Reqs/sec      6992.06    5483.39   67595.10
    Latency        7.14ms     3.79ms   348.28ms
    HTTP codes:
      1xx - 0, 2xx - 91149, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8851
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8851
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
    Reqs/sec     23635.15    7108.57   34937.30
    Latency        2.11ms     2.05ms   185.58ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.36MB/s
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
    Reqs/sec     20350.49    6015.60   29998.04
    Latency        2.45ms     2.02ms   183.27ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.60MB/s
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
    Reqs/sec     62946.85    3715.76   65432.24
    Latency      791.66us    67.72us     3.01ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.94MB/s
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
    Reqs/sec     19121.70    8758.68   74265.83
    Latency        2.61ms     2.42ms   212.57ms
    HTTP codes:
      1xx - 0, 2xx - 91952, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8048
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8048
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
    Reqs/sec     25451.79    7707.40   58589.47
    Latency        1.96ms     1.96ms   166.57ms
    HTTP codes:
      1xx - 0, 2xx - 97592, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2408
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2408
    Throughput:     5.69MB/s
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
    Reqs/sec     75694.60    3175.92   85283.51
    Latency      657.57us   172.70us     9.61ms
    HTTP codes:
      1xx - 0, 2xx - 96464, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3536
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3536
    Throughput:    11.56MB/s
  ```


