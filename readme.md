## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `71944` | `5107` | `81082` |
| **85%** | [Hyper Express](#hyper-express) | `60793` | `3588` | `64339` |
| **35%** | [Node (Default)](#node-default) | `25130` | `7648` | `61825` |
| **34%** | [Fastify](#fastify) | `24174` | `7884` | `36459` |
| **29%** | [Hono](#hono) | `20791` | `6056` | `30392` |
| **26%** | [Koa](#koa) | `18543` | `6822` | `57269` |
| **11%** | [Carbon](#carbon) | `8167` | `1417` | `10253` |
| **9%** | [Express](#express) | `6300` | `1064` | `10010` |


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
    Reqs/sec      8762.46    4850.71   57168.49
    Latency        5.70ms     4.44ms   381.35ms
    HTTP codes:
      1xx - 0, 2xx - 92682, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7318
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7318
    Throughput:     1.84MB/s
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
    Reqs/sec      6173.70     986.25    8241.48
    Latency        8.10ms     3.65ms   355.91ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.77MB/s
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
    Reqs/sec     24040.33    7617.15   36841.94
    Latency        2.08ms     2.04ms   187.11ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.46MB/s
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
    Reqs/sec     20147.30    5593.21   29930.22
    Latency        2.48ms     2.02ms   183.26ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.55MB/s
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
    Reqs/sec     61527.75    3491.11   66191.65
    Latency      810.94us    70.79us     2.65ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.73MB/s
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
    Reqs/sec     18940.79    6972.45   56251.50
    Latency        2.63ms     2.37ms   209.19ms
    HTTP codes:
      1xx - 0, 2xx - 93530, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6470
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6470
    Throughput:     4.01MB/s
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
    Reqs/sec     25272.57    7258.16   49304.54
    Latency        1.97ms     1.91ms   168.38ms
    HTTP codes:
      1xx - 0, 2xx - 97901, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2099
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2099
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
    Reqs/sec     72506.85    5108.43   79136.63
    Latency      686.05us   227.04us    11.28ms
    HTTP codes:
      1xx - 0, 2xx - 96460, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3540
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3540
    Throughput:    11.07MB/s
  ```


