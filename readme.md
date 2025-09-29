## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74298` | `2376` | `77145` |
| **82%** | [Hyper Express](#hyper-express) | `61248` | `3774` | `66917` |
| **34%** | [Node (Default)](#node-default) | `24908` | `7283` | `61564` |
| **33%** | [Fastify](#fastify) | `24340` | `7743` | `35457` |
| **28%** | [Hono](#hono) | `21058` | `6030` | `29806` |
| **24%** | [Koa](#koa) | `18091` | `7380` | `66283` |
| **11%** | [Carbon](#carbon) | `8416` | `1481` | `10449` |
| **8%** | [Express](#express) | `6265` | `974` | `8268` |


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
    Reqs/sec      8795.93    5667.18   76208.70
    Latency        5.67ms     4.25ms   367.30ms
    HTTP codes:
      1xx - 0, 2xx - 92469, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7531
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7531
    Throughput:     1.85MB/s
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
    Reqs/sec      6375.12    1057.17    8275.06
    Latency        7.84ms     3.63ms   350.46ms
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
    Reqs/sec     23305.69    6753.70   35411.67
    Latency        2.14ms     1.98ms   176.84ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.29MB/s
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
    Reqs/sec     20464.46    5614.98   30356.64
    Latency        2.44ms     2.07ms   185.30ms
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
    Reqs/sec     62296.32    4254.45   65625.17
    Latency      798.95us    84.15us     2.96ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.87MB/s
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
    Reqs/sec     18988.20    8758.98   82526.49
    Latency        2.63ms     2.46ms   214.81ms
    HTTP codes:
      1xx - 0, 2xx - 91865, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8135
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8135
    Throughput:     3.94MB/s
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
    Reqs/sec     27544.15    9063.70   64913.47
    Latency        1.81ms     1.91ms   165.60ms
    HTTP codes:
      1xx - 0, 2xx - 97386, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2614
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2614
    Throughput:     6.14MB/s
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
    Reqs/sec     74619.84    3652.71   89398.89
    Latency      667.18us   139.02us     9.22ms
    HTTP codes:
      1xx - 0, 2xx - 96629, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3371
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3371
    Throughput:    11.41MB/s
  ```


