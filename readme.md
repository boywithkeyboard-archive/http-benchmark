## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74143` | `3412` | `82032` |
| **85%** | [Hyper Express](#hyper-express) | `63280` | `3791` | `72636` |
| **34%** | [Node (Default)](#node-default) | `25475` | `7917` | `74881` |
| **30%** | [Fastify](#fastify) | `22258` | `5725` | `36459` |
| **26%** | [Hono](#hono) | `19034` | `4556` | `30148` |
| **24%** | [Koa](#koa) | `17739` | `8090` | `74638` |
| **11%** | [Carbon](#carbon) | `8378` | `1501` | `10416` |
| **9%** | [Express](#express) | `6412` | `1089` | `8544` |


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
    Reqs/sec      9026.14    6214.17   80948.35
    Latency        5.52ms     4.28ms   370.27ms
    HTTP codes:
      1xx - 0, 2xx - 91223, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8777
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8777
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
    Reqs/sec      6437.21    1097.23    8486.46
    Latency        7.76ms     3.52ms   342.25ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.84MB/s
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
    Reqs/sec     22074.06    5652.80   36149.24
    Latency        2.26ms     2.04ms   181.42ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.01MB/s
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
    Reqs/sec     19370.86    4597.17   29385.12
    Latency        2.58ms     2.07ms   184.68ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.38MB/s
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
    Reqs/sec     63357.98    3911.40   68642.55
    Latency      786.99us    80.86us     3.26ms
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
    Reqs/sec     17539.31    7577.96   66898.67
    Latency        2.85ms     2.38ms   210.21ms
    HTTP codes:
      1xx - 0, 2xx - 92792, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7208
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7208
    Throughput:     3.68MB/s
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
    Reqs/sec     25579.58    7812.93   68066.48
    Latency        1.95ms     1.80ms   156.30ms
    HTTP codes:
      1xx - 0, 2xx - 96206, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3794
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3794
    Throughput:     5.64MB/s
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
    Reqs/sec     75359.60    3106.29   84001.20
    Latency      660.34us   170.49us     8.02ms
    HTTP codes:
      1xx - 0, 2xx - 96594, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3406
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3406
    Throughput:    11.52MB/s
  ```


