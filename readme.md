## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74818` | `6642` | `85560` |
| **84%** | [Hyper Express](#hyper-express) | `62579` | `3084` | `69212` |
| **35%** | [Node (Default)](#node-default) | `26471` | `8659` | `57168` |
| **33%** | [Fastify](#fastify) | `24771` | `8157` | `35867` |
| **27%** | [Hono](#hono) | `20466` | `5646` | `29658` |
| **23%** | [Koa](#koa) | `17327` | `6144` | `56894` |
| **11%** | [Carbon](#carbon) | `8036` | `1368` | `10278` |
| **8%** | [Express](#express) | `6355` | `983` | `8406` |


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
    Reqs/sec      8384.29    3725.83   58457.62
    Latency        5.94ms     4.18ms   366.22ms
    HTTP codes:
      1xx - 0, 2xx - 94859, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5141
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5141
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
    Reqs/sec      6478.05    1023.74    8411.92
    Latency        7.71ms     3.52ms   337.55ms
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
    Reqs/sec     23600.03    6712.05   35707.55
    Latency        2.12ms     1.92ms   172.85ms
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
    Reqs/sec     20604.71    6032.00   29879.84
    Latency        2.42ms     1.98ms   179.27ms
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
    Reqs/sec     63373.89    3868.11   79252.14
    Latency      786.83us    83.44us     3.41ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.00MB/s
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
    Reqs/sec     17295.53    6227.82   56010.97
    Latency        2.88ms     2.32ms   206.41ms
    HTTP codes:
      1xx - 0, 2xx - 93619, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6381
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6381
    Throughput:     3.66MB/s
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
    Reqs/sec     26795.09    8275.92   46264.78
    Latency        1.86ms     1.93ms   163.26ms
    HTTP codes:
      1xx - 0, 2xx - 98336, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1664
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1664
    Throughput:     6.03MB/s
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
    Reqs/sec     73962.39    7361.50  108647.95
    Latency      678.37us   196.54us    13.48ms
    HTTP codes:
      1xx - 0, 2xx - 98256, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1744
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1744
    Throughput:    11.42MB/s
  ```


