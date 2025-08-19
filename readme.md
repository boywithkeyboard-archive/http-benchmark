## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `76671` | `2272` | `81590` |
| **82%** | [Hyper Express](#hyper-express) | `63097` | `4211` | `65944` |
| **35%** | [Node (Default)](#node-default) | `26848` | `8098` | `58506` |
| **32%** | [Fastify](#fastify) | `24891` | `8218` | `37781` |
| **29%** | [Hono](#hono) | `22383` | `6663` | `31163` |
| **25%** | [Koa](#koa) | `18929` | `9061` | `79234` |
| **11%** | [Carbon](#carbon) | `8118` | `1435` | `10260` |
| **8%** | [Express](#express) | `6386` | `1045` | `8346` |


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
    Reqs/sec      8955.18    5590.65   71267.34
    Latency        5.57ms     4.19ms   361.20ms
    HTTP codes:
      1xx - 0, 2xx - 92227, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7773
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7773
    Throughput:     1.88MB/s
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
    Reqs/sec      7041.80    6109.78   69055.51
    Latency        7.10ms     3.84ms   350.50ms
    HTTP codes:
      1xx - 0, 2xx - 89859, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10141
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10141
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
    Reqs/sec     24931.79    8186.11   36684.07
    Latency        2.00ms     1.95ms   174.59ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.66MB/s
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
    Reqs/sec     20091.20    5326.08   29344.36
    Latency        2.49ms     2.05ms   185.27ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.54MB/s
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
    Reqs/sec     61939.14    3126.46   64812.54
    Latency      804.88us    78.84us     3.26ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.80MB/s
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
    Reqs/sec     19295.38   10087.45   80656.34
    Latency        2.59ms     2.51ms   219.95ms
    HTTP codes:
      1xx - 0, 2xx - 89898, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10102
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10102
    Throughput:     3.92MB/s
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
    Reqs/sec     25165.88    7690.29   59745.45
    Latency        1.98ms     1.79ms   155.31ms
    HTTP codes:
      1xx - 0, 2xx - 97511, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2489
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2489
    Throughput:     5.62MB/s
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
    Reqs/sec     76016.62    3123.95   88570.53
    Latency      654.58us   162.18us    11.32ms
    HTTP codes:
      1xx - 0, 2xx - 95868, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4132
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4132
    Throughput:    11.54MB/s
  ```


