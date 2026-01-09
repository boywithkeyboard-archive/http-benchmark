## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74882` | `2897` | `84307` |
| **83%** | [Hyper Express](#hyper-express) | `62277` | `3293` | `65765` |
| **34%** | [Node (Default)](#node-default) | `25594` | `7898` | `57381` |
| **32%** | [Fastify](#fastify) | `24000` | `7710` | `35655` |
| **27%** | [Hono](#hono) | `19928` | `5491` | `28447` |
| **24%** | [Koa](#koa) | `18123` | `9039` | `81212` |
| **11%** | [Carbon](#carbon) | `8227` | `1459` | `10348` |
| **8%** | [Express](#express) | `6311` | `1048` | `8259` |


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
    Reqs/sec      9014.31    6517.15   76697.43
    Latency        5.53ms     4.36ms   375.70ms
    HTTP codes:
      1xx - 0, 2xx - 90330, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9670
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9670
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
    Reqs/sec      6449.68    1116.49    8277.53
    Latency        7.75ms     3.76ms   358.38ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.84MB/s
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
    Reqs/sec     23675.13    7436.67   36923.15
    Latency        2.11ms     2.06ms   183.07ms
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
    Reqs/sec     20290.38    5523.97   29787.06
    Latency        2.46ms     2.07ms   183.99ms
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
    Reqs/sec     62064.48    3586.86   64478.01
    Latency      803.69us    71.51us     3.35ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.81MB/s
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
    Reqs/sec     18747.91    9066.08   79322.65
    Latency        2.66ms     2.39ms   211.30ms
    HTTP codes:
      1xx - 0, 2xx - 91682, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8318
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8318
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
    Reqs/sec     25607.99    8629.77   80451.38
    Latency        1.95ms     1.83ms   155.18ms
    HTTP codes:
      1xx - 0, 2xx - 96520, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3480
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3480
    Throughput:     5.66MB/s
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
    Reqs/sec     75040.72    3147.11   81190.97
    Latency      664.15us   153.73us     7.16ms
    HTTP codes:
      1xx - 0, 2xx - 97017, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2983
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2983
    Throughput:    11.52MB/s
  ```


