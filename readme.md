## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75265` | `6432` | `83606` |
| **86%** | [Hyper Express](#hyper-express) | `64662` | `3895` | `72640` |
| **34%** | [Node (Default)](#node-default) | `25741` | `7541` | `39770` |
| **31%** | [Fastify](#fastify) | `22968` | `6591` | `35725` |
| **29%** | [Hono](#hono) | `21536` | `6329` | `31419` |
| **25%** | [Koa](#koa) | `18547` | `6423` | `48677` |
| **11%** | [Carbon](#carbon) | `8375` | `1414` | `10705` |
| **9%** | [Express](#express) | `6702` | `1092` | `8555` |


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
    Reqs/sec      8857.54    3651.86   54825.32
    Latency        5.63ms     4.11ms   354.17ms
    HTTP codes:
      1xx - 0, 2xx - 95334, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4666
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4666
    Throughput:     1.92MB/s
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
    Reqs/sec      7092.29    3533.82   56123.38
    Latency        7.04ms     3.90ms   354.42ms
    HTTP codes:
      1xx - 0, 2xx - 93544, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6456
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6451
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 5
    Throughput:     1.90MB/s
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
    Reqs/sec     24058.40    7317.46   36230.15
    Latency        2.08ms     1.98ms   177.16ms
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
    Reqs/sec     21860.11    6004.81   30214.45
    Latency        2.28ms     1.91ms   172.52ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.94MB/s
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
    Reqs/sec     63921.89    3856.70   69135.61
    Latency      779.60us    66.18us     2.87ms
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
    Reqs/sec     18471.02    6569.28   61118.36
    Latency        2.70ms     2.25ms   203.79ms
    HTTP codes:
      1xx - 0, 2xx - 94010, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5990
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5990
    Throughput:     3.92MB/s
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
    Reqs/sec     27030.69    7983.47   56789.77
    Latency        1.84ms     1.94ms   166.17ms
    HTTP codes:
      1xx - 0, 2xx - 97196, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2804
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2804
    Throughput:     6.02MB/s
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
    Reqs/sec     75436.36    6351.63   83448.81
    Latency      660.68us   194.54us     9.77ms
    HTTP codes:
      1xx - 0, 2xx - 97968, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2032
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2032
    Throughput:    11.69MB/s
  ```


