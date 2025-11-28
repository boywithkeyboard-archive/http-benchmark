## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74780` | `3071` | `82908` |
| **83%** | [Hyper Express](#hyper-express) | `61991` | `3657` | `65164` |
| **34%** | [Node (Default)](#node-default) | `25578` | `8072` | `62776` |
| **33%** | [Fastify](#fastify) | `24445` | `7677` | `35309` |
| **28%** | [Hono](#hono) | `20566` | `5571` | `30501` |
| **26%** | [Koa](#koa) | `19456` | `9581` | `81170` |
| **11%** | [Carbon](#carbon) | `8247` | `1506` | `10405` |
| **9%** | [Express](#express) | `6368` | `1069` | `8357` |


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
    Reqs/sec      8997.20    5628.42   66804.73
    Latency        5.55ms     4.26ms   366.91ms
    HTTP codes:
      1xx - 0, 2xx - 92270, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7730
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7730
    Throughput:     1.89MB/s
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
    Reqs/sec      6960.27    6024.12   73432.96
    Latency        7.16ms     3.96ms   363.84ms
    HTTP codes:
      1xx - 0, 2xx - 89709, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10291
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10291
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
    Reqs/sec     23110.09    6734.52   35429.98
    Latency        2.16ms     2.04ms   183.18ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.24MB/s
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
    Reqs/sec     21502.11    5843.94   29795.99
    Latency        2.32ms     1.99ms   177.71ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.86MB/s
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
    Reqs/sec     62529.32    3479.68   65037.39
    Latency      797.42us    76.28us     3.10ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.88MB/s
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
    Reqs/sec     18992.36    8803.50   74473.41
    Latency        2.63ms     2.31ms   204.72ms
    HTTP codes:
      1xx - 0, 2xx - 91935, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8065
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8065
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
    Reqs/sec     25862.74    8722.01   65155.99
    Latency        1.93ms     2.02ms   172.24ms
    HTTP codes:
      1xx - 0, 2xx - 97319, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2681
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2681
    Throughput:     5.76MB/s
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
    Reqs/sec     74697.96    2857.95   87713.24
    Latency      668.00us   169.00us     8.32ms
    HTTP codes:
      1xx - 0, 2xx - 96604, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3396
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3396
    Throughput:    11.39MB/s
  ```


