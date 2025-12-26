## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74121` | `3500` | `82196` |
| **81%** | [Hyper Express](#hyper-express) | `60324` | `3963` | `66296` |
| **35%** | [Fastify](#fastify) | `25581` | `8260` | `35612` |
| **34%** | [Node (Default)](#node-default) | `25034` | `7588` | `57741` |
| **27%** | [Hono](#hono) | `20117` | `5467` | `28472` |
| **25%** | [Koa](#koa) | `18877` | `8014` | `66047` |
| **11%** | [Carbon](#carbon) | `8008` | `1431` | `10235` |
| **8%** | [Express](#express) | `6146` | `979` | `8189` |


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
    Reqs/sec      8887.81    6147.01   75677.80
    Latency        5.61ms     4.39ms   380.48ms
    HTTP codes:
      1xx - 0, 2xx - 91050, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8950
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8950
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
    Reqs/sec      6243.36    1001.69    8258.44
    Latency        8.00ms     3.69ms   355.27ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     23708.93    7177.48   36051.22
    Latency        2.11ms     1.98ms   179.75ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.38MB/s
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
    Reqs/sec     19925.66    5206.60   29236.26
    Latency        2.51ms     2.07ms   188.32ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.50MB/s
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
    Reqs/sec     61078.06    3384.14   65760.86
    Latency      816.30us    70.57us     2.88ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.68MB/s
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
    Reqs/sec     18714.19    8248.47   72608.18
    Latency        2.67ms     2.45ms   215.59ms
    HTTP codes:
      1xx - 0, 2xx - 91856, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8144
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8144
    Throughput:     3.88MB/s
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
    Reqs/sec     25258.70    8307.82   71252.16
    Latency        1.97ms     2.00ms   168.47ms
    HTTP codes:
      1xx - 0, 2xx - 96532, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3468
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3468
    Throughput:     5.58MB/s
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
    Reqs/sec     74000.95    3000.61   83741.36
    Latency      672.65us   172.63us     9.27ms
    HTTP codes:
      1xx - 0, 2xx - 96244, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3756
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3756
    Throughput:    11.27MB/s
  ```


