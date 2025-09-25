## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74964` | `3355` | `82080` |
| **84%** | [Hyper Express](#hyper-express) | `62944` | `3771` | `64890` |
| **36%** | [Node (Default)](#node-default) | `26975` | `9275` | `73304` |
| **32%** | [Fastify](#fastify) | `23950` | `7279` | `34850` |
| **27%** | [Hono](#hono) | `20355` | `5448` | `28473` |
| **25%** | [Koa](#koa) | `18965` | `8563` | `71200` |
| **11%** | [Carbon](#carbon) | `8119` | `1447` | `10172` |
| **8%** | [Express](#express) | `6216` | `1006` | `8050` |


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
    Reqs/sec      8898.37    7254.46   81087.55
    Latency        5.60ms     4.44ms   381.90ms
    HTTP codes:
      1xx - 0, 2xx - 88591, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11409
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11409
    Throughput:     1.79MB/s
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
    Reqs/sec      6939.33    6020.91   75078.27
    Latency        7.20ms     4.15ms   374.46ms
    HTTP codes:
      1xx - 0, 2xx - 89893, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10107
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10107
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
    Reqs/sec     22787.48    6836.08   34492.84
    Latency        2.19ms     2.08ms   187.35ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.17MB/s
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
    Reqs/sec     20874.65    5922.37   29518.97
    Latency        2.39ms     2.09ms   187.68ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.72MB/s
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
    Reqs/sec     62987.71    4760.86   79585.87
    Latency      791.33us   113.83us     4.96ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.95MB/s
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
    Reqs/sec     18153.57    7713.40   68080.34
    Latency        2.75ms     2.43ms   212.90ms
    HTTP codes:
      1xx - 0, 2xx - 92922, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7078
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7078
    Throughput:     3.82MB/s
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
    Reqs/sec     26333.54    8691.78   69640.72
    Latency        1.89ms     2.08ms   172.04ms
    HTTP codes:
      1xx - 0, 2xx - 96494, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3506
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3506
    Throughput:     5.82MB/s
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
    Reqs/sec     76589.30    3260.45   84124.99
    Latency      650.66us   138.89us     6.24ms
    HTTP codes:
      1xx - 0, 2xx - 97399, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2601
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2601
    Throughput:    11.79MB/s
  ```


