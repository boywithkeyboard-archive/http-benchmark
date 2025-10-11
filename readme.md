## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73526` | `3036` | `80450` |
| **84%** | [Hyper Express](#hyper-express) | `61445` | `3838` | `64345` |
| **33%** | [Node (Default)](#node-default) | `24117` | `7614` | `78904` |
| **33%** | [Fastify](#fastify) | `24008` | `7652` | `35634` |
| **28%** | [Hono](#hono) | `20256` | `5593` | `29448` |
| **27%** | [Koa](#koa) | `19745` | `8681` | `77150` |
| **11%** | [Carbon](#carbon) | `8180` | `1426` | `10349` |
| **9%** | [Express](#express) | `6371` | `1060` | `8349` |


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
    Reqs/sec      8945.78    5877.97   75779.42
    Latency        5.58ms     4.33ms   373.58ms
    HTTP codes:
      1xx - 0, 2xx - 91976, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8024
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8024
    Throughput:     1.87MB/s
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
    Reqs/sec      7051.94    6295.75   77564.94
    Latency        7.08ms     3.98ms   363.23ms
    HTTP codes:
      1xx - 0, 2xx - 89544, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10456
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10456
    Throughput:     1.81MB/s
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
    Reqs/sec     23548.56    7189.64   35344.45
    Latency        2.12ms     1.98ms   178.99ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.34MB/s
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
    Reqs/sec     20423.95    5558.06   29782.98
    Latency        2.45ms     2.03ms   187.14ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.61MB/s
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
    Reqs/sec     62678.38    2810.56   65295.29
    Latency      795.41us    70.05us     2.92ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.90MB/s
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
    Reqs/sec     18478.42    8312.06   69582.76
    Latency        2.70ms     2.35ms   210.80ms
    HTTP codes:
      1xx - 0, 2xx - 92243, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7757
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7757
    Throughput:     3.85MB/s
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
    Reqs/sec     24979.36    7415.04   60872.94
    Latency        2.00ms     1.82ms   158.72ms
    HTTP codes:
      1xx - 0, 2xx - 97451, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2549
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2549
    Throughput:     5.58MB/s
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
    Reqs/sec     74683.71    2755.67   83099.35
    Latency      666.32us   167.45us     9.96ms
    HTTP codes:
      1xx - 0, 2xx - 96556, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3444
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3444
    Throughput:    11.41MB/s
  ```


