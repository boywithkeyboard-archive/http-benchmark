## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72673` | `4725` | `80950` |
| **84%** | [Hyper Express](#hyper-express) | `61181` | `3658` | `81771` |
| **36%** | [Node (Default)](#node-default) | `25900` | `8054` | `56170` |
| **33%** | [Fastify](#fastify) | `24239` | `6693` | `35783` |
| **29%** | [Hono](#hono) | `21297` | `5808` | `29701` |
| **25%** | [Koa](#koa) | `18309` | `6761` | `58369` |
| **11%** | [Carbon](#carbon) | `8253` | `1443` | `10203` |
| **9%** | [Express](#express) | `6561` | `1081` | `8502` |


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
    Reqs/sec      8819.79    4591.06   51399.68
    Latency        5.65ms     4.26ms   366.01ms
    HTTP codes:
      1xx - 0, 2xx - 92657, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7343
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7343
    Throughput:     1.86MB/s
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
    Reqs/sec      6560.69    1129.20    8443.65
    Latency        7.62ms     3.62ms   346.52ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.88MB/s
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
    Reqs/sec     24310.31    7385.82   36393.06
    Latency        2.05ms     2.01ms   182.27ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.52MB/s
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
    Reqs/sec     20994.65    5581.53   29321.27
    Latency        2.38ms     2.05ms   184.28ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.74MB/s
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
    Reqs/sec     61030.82    2972.76   66353.10
    Latency      817.21us    66.25us     2.94ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.67MB/s
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
    Reqs/sec     19867.29    7319.92   59490.44
    Latency        2.51ms     2.41ms   211.78ms
    HTTP codes:
      1xx - 0, 2xx - 93231, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6769
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6769
    Throughput:     4.19MB/s
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
    Reqs/sec     26227.79    7568.53   47017.02
    Latency        1.90ms     1.93ms   167.84ms
    HTTP codes:
      1xx - 0, 2xx - 98069, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1931
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1931
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
    Reqs/sec     72090.97    6110.31   81562.82
    Latency      690.53us   263.60us    14.88ms
    HTTP codes:
      1xx - 0, 2xx - 96884, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3116
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3116
    Throughput:    11.04MB/s
  ```


