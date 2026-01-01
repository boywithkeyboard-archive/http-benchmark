## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75431` | `2576` | `85378` |
| **83%** | [Hyper Express](#hyper-express) | `62581` | `3717` | `66836` |
| **36%** | [Node (Default)](#node-default) | `26995` | `8666` | `79037` |
| **31%** | [Fastify](#fastify) | `23390` | `6568` | `36516` |
| **28%** | [Hono](#hono) | `20914` | `5598` | `30094` |
| **26%** | [Koa](#koa) | `19334` | `10330` | `82596` |
| **11%** | [Carbon](#carbon) | `8254` | `1418` | `10375` |
| **9%** | [Express](#express) | `6528` | `1103` | `8427` |


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
    Reqs/sec      8978.26    6677.22   80412.07
    Latency        5.56ms     4.23ms   364.32ms
    HTTP codes:
      1xx - 0, 2xx - 90758, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9242
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9242
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
    Reqs/sec      7255.73    6473.08   80182.71
    Latency        6.88ms     3.96ms   361.33ms
    HTTP codes:
      1xx - 0, 2xx - 88994, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11006
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11006
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
    Reqs/sec     25334.00    8024.74   36073.56
    Latency        1.97ms     1.99ms   178.60ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.76MB/s
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
    Reqs/sec     20948.12    5889.67   29663.45
    Latency        2.38ms     2.11ms   187.54ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.73MB/s
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
    Reqs/sec     63660.51    3834.90   66752.18
    Latency      783.14us    64.70us     2.83ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.04MB/s
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
    Reqs/sec     18871.44    8310.43   70183.42
    Latency        2.64ms     2.37ms   210.19ms
    HTTP codes:
      1xx - 0, 2xx - 92313, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7687
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7687
    Throughput:     3.94MB/s
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
    Reqs/sec     26674.86    8459.18   63553.80
    Latency        1.87ms     1.95ms   167.80ms
    HTTP codes:
      1xx - 0, 2xx - 97425, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2575
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2575
    Throughput:     5.95MB/s
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
    Reqs/sec     75409.08    2721.48   82788.53
    Latency      660.70us   138.20us     5.34ms
    HTTP codes:
      1xx - 0, 2xx - 96975, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3025
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3025
    Throughput:    11.57MB/s
  ```


