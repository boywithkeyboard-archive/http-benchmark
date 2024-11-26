## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73352` | `7054` | `81370` |
| **85%** | [Hyper Express](#hyper-express) | `62009` | `4917` | `76410` |
| **35%** | [Node (Default)](#node-default) | `25827` | `7854` | `66092` |
| **34%** | [Fastify](#fastify) | `24963` | `7717` | `37063` |
| **28%** | [Hono](#hono) | `20508` | `5619` | `29727` |
| **25%** | [Koa](#koa) | `18484` | `6578` | `58770` |
| **11%** | [Carbon](#carbon) | `8428` | `1443` | `10549` |
| **9%** | [Express](#express) | `6566` | `1085` | `8581` |


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
    Reqs/sec      8654.10    4106.84   49619.19
    Latency        5.77ms     4.20ms   365.43ms
    HTTP codes:
      1xx - 0, 2xx - 94248, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5752
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5752
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
    Reqs/sec      6467.59     993.59    8589.36
    Latency        7.73ms     3.47ms   332.40ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.85MB/s
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
    Reqs/sec     23945.28    7082.85   35802.64
    Latency        2.08ms     2.05ms   185.81ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.44MB/s
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
    Reqs/sec     20985.17    5634.56   29674.87
    Latency        2.38ms     2.03ms   183.29ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.74MB/s
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
    Reqs/sec     63680.94    4184.79   69621.28
    Latency      782.84us    80.78us     3.21ms
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
    Reqs/sec     18110.49    5839.80   43135.88
    Latency        2.75ms     2.38ms   207.95ms
    HTTP codes:
      1xx - 0, 2xx - 95666, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4334
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4334
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
    Reqs/sec     25974.24    8413.67   61902.68
    Latency        1.92ms     1.91ms   159.35ms
    HTTP codes:
      1xx - 0, 2xx - 95378, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4622
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4622
    Throughput:     5.67MB/s
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
    Reqs/sec     73917.81    5481.17   77289.58
    Latency      674.88us   178.03us    11.25ms
    HTTP codes:
      1xx - 0, 2xx - 97230, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2770
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2770
    Throughput:    11.36MB/s
  ```


