## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73976` | `5659` | `83319` |
| **84%** | [Hyper Express](#hyper-express) | `62494` | `3878` | `69155` |
| **38%** | [Node (Default)](#node-default) | `27756` | `8044` | `45009` |
| **34%** | [Fastify](#fastify) | `25075` | `7613` | `36783` |
| **29%** | [Hono](#hono) | `21169` | `5853` | `30621` |
| **26%** | [Koa](#koa) | `19270` | `6710` | `52722` |
| **12%** | [Carbon](#carbon) | `8527` | `1493` | `10671` |
| **9%** | [Express](#express) | `6643` | `1085` | `8643` |


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
    Reqs/sec      8866.68    4865.42   59538.20
    Latency        5.63ms     4.15ms   359.48ms
    HTTP codes:
      1xx - 0, 2xx - 93101, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6899
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6899
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
    Reqs/sec      6907.05    4299.04   56746.25
    Latency        7.23ms     3.84ms   348.35ms
    HTTP codes:
      1xx - 0, 2xx - 92548, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7452
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7452
    Throughput:     1.83MB/s
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
    Reqs/sec     25802.63    8115.20   38054.01
    Latency        1.94ms     2.01ms   182.14ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.85MB/s
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
    Reqs/sec     21520.85    6100.00   30467.63
    Latency        2.32ms     2.06ms   180.23ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.86MB/s
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
    Reqs/sec     62421.66    3963.18   69188.98
    Latency      799.10us    87.46us     4.32ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.86MB/s
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
    Reqs/sec     19644.33    7096.68   57882.67
    Latency        2.54ms     2.16ms   187.59ms
    HTTP codes:
      1xx - 0, 2xx - 93966, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6034
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6034
    Throughput:     4.17MB/s
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
    Reqs/sec     26472.45    7232.91   51724.57
    Latency        1.88ms     1.85ms   160.85ms
    HTTP codes:
      1xx - 0, 2xx - 97215, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2785
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2785
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
    Reqs/sec     74115.50    7292.95   83948.69
    Latency      672.31us   238.99us     8.62ms
    HTTP codes:
      1xx - 0, 2xx - 97341, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2659
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2659
    Throughput:    11.42MB/s
  ```


