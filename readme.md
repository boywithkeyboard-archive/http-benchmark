## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75554` | `2599` | `80946` |
| **86%** | [Hyper Express](#hyper-express) | `65135` | `3976` | `69202` |
| **44%** | [Node (Default)](#node-default) | `33421` | `8384` | `72912` |
| **43%** | [Fastify](#fastify) | `32514` | `8209` | `50797` |
| **39%** | [Hono](#hono) | `29134` | `7904` | `46336` |
| **37%** | [Koa](#koa) | `28293` | `12091` | `72344` |
| **12%** | [Carbon](#carbon) | `9313` | `2403` | `13287` |
| **10%** | [Express](#express) | `7371` | `1794` | `10751` |


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
    Reqs/sec      9820.84    5986.04   71833.32
    Latency        5.08ms     4.29ms   372.95ms
    HTTP codes:
      1xx - 0, 2xx - 92626, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7374
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7374
    Throughput:     2.07MB/s
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
    Reqs/sec      7976.58    6735.06   75614.14
    Latency        6.26ms     3.94ms   362.35ms
    HTTP codes:
      1xx - 0, 2xx - 89489, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10511
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10511
    Throughput:     2.04MB/s
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
    Reqs/sec     31988.03    7904.76   48591.70
    Latency        1.56ms     1.91ms   170.08ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.27MB/s
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
    Reqs/sec     28653.60    7521.02   46022.11
    Latency        1.74ms     2.01ms   181.59ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     6.47MB/s
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
    Reqs/sec     65301.75    3702.04   67835.43
    Latency      763.72us    96.87us     4.49ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.28MB/s
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
    Reqs/sec     27717.37   11581.48   77585.73
    Latency        1.80ms     2.34ms   205.13ms
    HTTP codes:
      1xx - 0, 2xx - 90826, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9174
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9174
    Throughput:     5.69MB/s
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
    Reqs/sec     33978.85    9225.97   80721.92
    Latency        1.47ms     1.90ms   161.77ms
    HTTP codes:
      1xx - 0, 2xx - 96600, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3400
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3400
    Throughput:     7.51MB/s
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
    Reqs/sec     74939.63    2376.57   80323.94
    Latency      664.22us   202.84us    11.12ms
    HTTP codes:
      1xx - 0, 2xx - 95386, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4614
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4614
    Throughput:    11.32MB/s
  ```


