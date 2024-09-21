## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75058` | `6223` | `88804` |
| **82%** | [Hyper Express](#hyper-express) | `61533` | `3604` | `64899` |
| **35%** | [Node (Default)](#node-default) | `26169` | `7776` | `48062` |
| **31%** | [Fastify](#fastify) | `23566` | `6709` | `36045` |
| **28%** | [Hono](#hono) | `20813` | `5464` | `29538` |
| **25%** | [Koa](#koa) | `18521` | `6785` | `52000` |
| **11%** | [Carbon](#carbon) | `8307` | `1444` | `10429` |
| **8%** | [Express](#express) | `6284` | `963` | `8347` |


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
    Reqs/sec      8855.66    5386.51   61774.03
    Latency        5.64ms     4.33ms   376.18ms
    HTTP codes:
      1xx - 0, 2xx - 91653, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8347
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8347
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
    Reqs/sec      6375.78    1062.47    8434.57
    Latency        7.84ms     3.87ms   365.71ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.82MB/s
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
    Reqs/sec     24406.76    7596.49   37342.65
    Latency        2.05ms     2.05ms   184.42ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.54MB/s
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
    Reqs/sec     21636.68    5582.93   30308.10
    Latency        2.31ms     2.05ms   183.55ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.89MB/s
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
    Reqs/sec     62610.64    3695.22   69605.05
    Latency      796.47us    76.85us     4.70ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.89MB/s
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
    Reqs/sec     19325.77    6944.82   54338.01
    Latency        2.58ms     2.39ms   207.98ms
    HTTP codes:
      1xx - 0, 2xx - 93373, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6627
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6627
    Throughput:     4.08MB/s
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
    Reqs/sec     26474.34    7528.74   53102.54
    Latency        1.89ms     1.85ms   161.20ms
    HTTP codes:
      1xx - 0, 2xx - 97297, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2703
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2703
    Throughput:     5.89MB/s
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
    Reqs/sec     74257.73    5588.39   84217.85
    Latency      670.27us   149.04us    10.90ms
    HTTP codes:
      1xx - 0, 2xx - 97475, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2525
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2525
    Throughput:    11.45MB/s
  ```


