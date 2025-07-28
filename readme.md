## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74886` | `3360` | `86087` |
| **83%** | [Hyper Express](#hyper-express) | `62065` | `3247` | `65348` |
| **34%** | [Node (Default)](#node-default) | `25776` | `8338` | `66119` |
| **32%** | [Fastify](#fastify) | `24229` | `7275` | `35571` |
| **28%** | [Hono](#hono) | `20834` | `5931` | `29619` |
| **25%** | [Koa](#koa) | `18464` | `7369` | `66365` |
| **11%** | [Carbon](#carbon) | `8322` | `1497` | `10492` |
| **9%** | [Express](#express) | `6485` | `1111` | `8526` |


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
    Reqs/sec      9177.32    6104.07   77680.19
    Latency        5.44ms     4.33ms   375.35ms
    HTTP codes:
      1xx - 0, 2xx - 91605, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8395
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8395
    Throughput:     1.91MB/s
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
    Reqs/sec      7036.74    6105.12   69468.49
    Latency        7.08ms     3.87ms   358.35ms
    HTTP codes:
      1xx - 0, 2xx - 89165, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10835
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10835
    Throughput:     1.80MB/s
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
    Reqs/sec     24308.05    7637.38   37282.62
    Latency        2.05ms     2.05ms   182.63ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.52MB/s
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
    Reqs/sec     20299.42    5814.11   29683.87
    Latency        2.46ms     2.03ms   182.39ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.59MB/s
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
    Reqs/sec     62535.75    3471.54   72566.91
    Latency      798.88us    70.96us     2.55ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.87MB/s
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
    Reqs/sec     19054.08    8647.16   77280.93
    Latency        2.62ms     2.39ms   208.74ms
    HTTP codes:
      1xx - 0, 2xx - 91758, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8242
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8242
    Throughput:     3.95MB/s
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
    Reqs/sec     26330.01    8573.48   76698.56
    Latency        1.89ms     1.96ms   168.58ms
    HTTP codes:
      1xx - 0, 2xx - 96655, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3345
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3345
    Throughput:     5.83MB/s
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
    Reqs/sec     75021.96    2380.08   79534.48
    Latency      663.43us   191.01us    10.77ms
    HTTP codes:
      1xx - 0, 2xx - 96457, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3543
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3543
    Throughput:    11.45MB/s
  ```


