## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `71264` | `3888` | `83431` |
| **84%** | [Hyper Express](#hyper-express) | `59539` | `3610` | `64148` |
| **30%** | [Node (Default)](#node-default) | `21124` | `5865` | `60646` |
| **30%** | [Hono](#hono) | `21104` | `6244` | `29321` |
| **28%** | [Fastify](#fastify) | `20178` | `5597` | `34287` |
| **25%** | [Koa](#koa) | `17470` | `8327` | `75395` |
| **11%** | [Carbon](#carbon) | `7516` | `1346` | `10134` |
| **9%** | [Express](#express) | `6092` | `1063` | `8166` |


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
    Reqs/sec      8106.99    6837.05   77854.59
    Latency        6.17ms     4.46ms   383.04ms
    HTTP codes:
      1xx - 0, 2xx - 88959, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11041
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11041
    Throughput:     1.64MB/s
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
    Reqs/sec      6151.24    1123.12    8175.33
    Latency        8.12ms     4.01ms   379.92ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.76MB/s
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
    Reqs/sec     19633.47    4747.44   34576.99
    Latency        2.55ms     2.07ms   187.50ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.45MB/s
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
    Reqs/sec     19681.56    6268.42   29630.45
    Latency        2.54ms     2.16ms   195.07ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.44MB/s
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
    Reqs/sec     59482.47    3769.04   70533.33
    Latency      839.77us    88.15us     3.47ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.43MB/s
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
    Reqs/sec     16968.71    7254.36   59702.90
    Latency        2.94ms     2.49ms   213.79ms
    HTTP codes:
      1xx - 0, 2xx - 93189, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6811
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6811
    Throughput:     3.58MB/s
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
    Reqs/sec     20001.68    5355.27   65005.83
    Latency        2.49ms     1.99ms   180.47ms
    HTTP codes:
      1xx - 0, 2xx - 96724, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3276
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3276
    Throughput:     4.43MB/s
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
    Reqs/sec     71406.34    4313.67   82968.62
    Latency      696.25us   224.84us    14.55ms
    HTTP codes:
      1xx - 0, 2xx - 95429, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4571
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4571
    Throughput:    10.79MB/s
  ```


