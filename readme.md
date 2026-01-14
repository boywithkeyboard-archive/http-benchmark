## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74255` | `2178` | `81354` |
| **84%** | [Hyper Express](#hyper-express) | `62062` | `3724` | `66177` |
| **34%** | [Node (Default)](#node-default) | `25089` | `7272` | `72457` |
| **33%** | [Fastify](#fastify) | `24280` | `7482` | `35746` |
| **28%** | [Hono](#hono) | `20880` | `6590` | `31061` |
| **26%** | [Koa](#koa) | `19175` | `8627` | `71922` |
| **11%** | [Carbon](#carbon) | `8084` | `1430` | `10310` |
| **9%** | [Express](#express) | `6507` | `1112` | `8403` |


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
    Reqs/sec      9190.29    7044.94   75846.65
    Latency        5.43ms     4.35ms   375.90ms
    HTTP codes:
      1xx - 0, 2xx - 89575, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10425
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10425
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
    Reqs/sec      7065.39    6158.37   75615.28
    Latency        7.07ms     3.94ms   362.23ms
    HTTP codes:
      1xx - 0, 2xx - 89540, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10460
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10460
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
    Reqs/sec     23601.86    7011.30   36285.34
    Latency        2.12ms     2.09ms   188.89ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.35MB/s
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
    Reqs/sec     20587.55    5634.01   29102.21
    Latency        2.43ms     2.09ms   189.94ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.65MB/s
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
    Reqs/sec     63423.63    3512.93   67657.21
    Latency      786.22us    66.31us     3.64ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.01MB/s
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
    Reqs/sec     18848.69    8150.05   74872.59
    Latency        2.64ms     2.40ms   215.12ms
    HTTP codes:
      1xx - 0, 2xx - 92706, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7294
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7294
    Throughput:     3.96MB/s
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
    Reqs/sec     26430.06    8787.55   65247.64
    Latency        1.89ms     1.96ms   170.84ms
    HTTP codes:
      1xx - 0, 2xx - 97209, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2791
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2791
    Throughput:     5.89MB/s
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
    Reqs/sec     74941.31    2593.05   82007.41
    Latency      664.92us   144.39us     7.22ms
    HTTP codes:
      1xx - 0, 2xx - 97454, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2546
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2546
    Throughput:    11.56MB/s
  ```


