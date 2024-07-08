## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `76022` | `6180` | `80583` |
| **84%** | [Hyper Express](#hyper-express) | `63856` | `3042` | `68096` |
| **35%** | [Node (Default)](#node-default) | `26864` | `8508` | `56176` |
| **31%** | [Fastify](#fastify) | `23654` | `6795` | `36539` |
| **27%** | [Hono](#hono) | `20472` | `5624` | `30577` |
| **23%** | [Koa](#koa) | `17774` | `6885` | `59678` |
| **11%** | [Carbon](#carbon) | `8115` | `1357` | `10392` |
| **9%** | [Express](#express) | `6582` | `1060` | `8506` |


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
    Reqs/sec      8626.74    4228.66   58381.68
    Latency        5.78ms     4.17ms   363.27ms
    HTTP codes:
      1xx - 0, 2xx - 94129, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5871
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5871
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
    Reqs/sec      6368.09     907.80    8450.78
    Latency        7.85ms     3.54ms   340.12ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.82MB/s
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
    Reqs/sec     24135.07    7678.51   36154.41
    Latency        2.07ms     2.05ms   185.03ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.47MB/s
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
    Reqs/sec     20514.45    5632.41   30215.06
    Latency        2.44ms     2.05ms   185.22ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.63MB/s
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
    Reqs/sec     63953.16    3464.45   67913.72
    Latency      780.15us    65.81us     2.75ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.08MB/s
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
    Reqs/sec     18112.22    5792.21   50172.18
    Latency        2.75ms     2.35ms   204.59ms
    HTTP codes:
      1xx - 0, 2xx - 95539, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4461
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4461
    Throughput:     3.91MB/s
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
    Reqs/sec     26248.89    8060.84   44540.80
    Latency        1.90ms     1.88ms   163.32ms
    HTTP codes:
      1xx - 0, 2xx - 98131, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1869
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1869
    Throughput:     5.90MB/s
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
    Reqs/sec     74730.90    6326.25   85997.08
    Latency      667.04us   204.32us    14.72ms
    HTTP codes:
      1xx - 0, 2xx - 97760, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2240
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2240
    Throughput:    11.56MB/s
  ```


