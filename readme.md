## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73909` | `3009` | `82084` |
| **84%** | [Hyper Express](#hyper-express) | `61718` | `2908` | `70042` |
| **34%** | [Node (Default)](#node-default) | `24915` | `9107` | `79637` |
| **31%** | [Fastify](#fastify) | `23077` | `7118` | `35534` |
| **29%** | [Hono](#hono) | `21210` | `6356` | `29727` |
| **25%** | [Koa](#koa) | `18444` | `8656` | `76417` |
| **11%** | [Carbon](#carbon) | `8201` | `1476` | `10504` |
| **9%** | [Express](#express) | `6334` | `1076` | `8348` |


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
    Reqs/sec      8908.39    5555.01   70423.10
    Latency        5.60ms     4.25ms   365.39ms
    HTTP codes:
      1xx - 0, 2xx - 92745, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7255
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7255
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
    Reqs/sec      6232.72    1046.46    8087.27
    Latency        8.02ms     3.76ms   360.91ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.78MB/s
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
    Reqs/sec     22402.27    6749.45   35063.82
    Latency        2.23ms     2.02ms   180.98ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.08MB/s
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
    Reqs/sec     20610.04    5669.60   28820.45
    Latency        2.42ms     2.06ms   182.14ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.66MB/s
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
    Reqs/sec     60956.56    3736.34   64764.28
    Latency      818.03us    78.82us     3.11ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.66MB/s
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
    Reqs/sec     18266.00    9589.07   80778.43
    Latency        2.73ms     2.36ms   205.68ms
    HTTP codes:
      1xx - 0, 2xx - 90215, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9785
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9785
    Throughput:     3.73MB/s
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
    Reqs/sec     23891.88    7564.19   78683.48
    Latency        2.09ms     2.00ms   171.37ms
    HTTP codes:
      1xx - 0, 2xx - 96624, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3376
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3376
    Throughput:     5.29MB/s
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
    Reqs/sec     74469.63    3397.30   86757.07
    Latency      669.45us   169.75us     9.43ms
    HTTP codes:
      1xx - 0, 2xx - 96318, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3682
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3682
    Throughput:    11.32MB/s
  ```


