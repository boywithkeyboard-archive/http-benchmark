## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74866` | `6354` | `98830` |
| **85%** | [Hyper Express](#hyper-express) | `63635` | `3654` | `72505` |
| **35%** | [Node (Default)](#node-default) | `25968` | `6683` | `45784` |
| **32%** | [Fastify](#fastify) | `24035` | `6892` | `37060` |
| **28%** | [Hono](#hono) | `20939` | `5669` | `30105` |
| **24%** | [Koa](#koa) | `18014` | `5878` | `48865` |
| **11%** | [Carbon](#carbon) | `8309` | `1385` | `10592` |
| **9%** | [Express](#express) | `6642` | `1062` | `8723` |


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
    Reqs/sec      8957.40    5494.71   66832.01
    Latency        5.57ms     4.10ms   356.14ms
    HTTP codes:
      1xx - 0, 2xx - 91443, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8557
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8557
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
    Reqs/sec      7074.54    3806.52   46492.50
    Latency        7.06ms     3.83ms   352.64ms
    HTTP codes:
      1xx - 0, 2xx - 93361, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6639
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6637
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 2
    Throughput:     1.89MB/s
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
    Reqs/sec     24825.45    7543.28   35926.12
    Latency        2.01ms     1.98ms   178.18ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.63MB/s
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
    Reqs/sec     21361.84    5792.07   30058.26
    Latency        2.34ms     1.92ms   173.47ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.83MB/s
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
    Reqs/sec     63833.71    4342.10   75778.41
    Latency      782.18us    69.97us     4.21ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.05MB/s
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
    Reqs/sec     18041.10    5855.29   52918.64
    Latency        2.75ms     2.35ms   203.53ms
    HTTP codes:
      1xx - 0, 2xx - 94659, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5341
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5341
    Throughput:     3.89MB/s
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
    Reqs/sec     26267.38    7182.01   52938.11
    Latency        1.90ms     1.76ms   152.91ms
    HTTP codes:
      1xx - 0, 2xx - 97465, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2535
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2535
    Throughput:     5.86MB/s
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
    Reqs/sec     76946.76    7238.29  116188.80
    Latency      651.95us   195.82us    13.73ms
    HTTP codes:
      1xx - 0, 2xx - 97101, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2899
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2899
    Throughput:    11.68MB/s
  ```


