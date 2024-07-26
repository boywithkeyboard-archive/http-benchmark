## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73174` | `4835` | `77675` |
| **88%** | [Hyper Express](#hyper-express) | `64421` | `7272` | `104684` |
| **34%** | [Node (Default)](#node-default) | `24952` | `7391` | `44191` |
| **33%** | [Fastify](#fastify) | `24002` | `7237` | `36414` |
| **28%** | [Hono](#hono) | `20335` | `5360` | `29568` |
| **25%** | [Koa](#koa) | `18326` | `5864` | `45361` |
| **11%** | [Carbon](#carbon) | `8033` | `1335` | `10316` |
| **9%** | [Express](#express) | `6578` | `1079` | `8511` |


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
    Reqs/sec      8453.28    4000.33   54164.84
    Latency        5.90ms     4.15ms   359.10ms
    HTTP codes:
      1xx - 0, 2xx - 94461, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5539
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5539
    Throughput:     1.81MB/s
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
    Reqs/sec      6538.84    1046.73    8506.85
    Latency        7.64ms     3.55ms   341.67ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.87MB/s
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
    Reqs/sec     23624.78    7189.34   36146.55
    Latency        2.11ms     1.97ms   176.57ms
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
    Reqs/sec     20790.61    5593.80   29582.32
    Latency        2.40ms     1.99ms   180.75ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.70MB/s
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
    Reqs/sec     63264.46    3870.83   70228.89
    Latency      787.97us    69.00us     3.24ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.98MB/s
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
    Reqs/sec     17914.72    6322.80   58944.34
    Latency        2.78ms     2.37ms   211.40ms
    HTTP codes:
      1xx - 0, 2xx - 94363, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5637
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5637
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
    Reqs/sec     25392.22    7527.83   41454.56
    Latency        1.97ms     1.81ms   162.95ms
    HTTP codes:
      1xx - 0, 2xx - 98158, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1842
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1841
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 1
    Throughput:     5.70MB/s
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
    Reqs/sec     72876.88    5475.46   79425.27
    Latency      684.17us   177.43us     7.21ms
    HTTP codes:
      1xx - 0, 2xx - 97447, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2553
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2553
    Throughput:    11.24MB/s
  ```


