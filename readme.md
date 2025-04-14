## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73282` | `4867` | `83815` |
| **85%** | [Hyper Express](#hyper-express) | `62497` | `4182` | `72540` |
| **38%** | [Node (Default)](#node-default) | `27581` | `8417` | `53070` |
| **33%** | [Fastify](#fastify) | `24259` | `7103` | `35655` |
| **28%** | [Hono](#hono) | `20300` | `4911` | `28860` |
| **26%** | [Koa](#koa) | `18742` | `6237` | `50265` |
| **11%** | [Carbon](#carbon) | `8168` | `1366` | `10306` |
| **9%** | [Express](#express) | `6382` | `997` | `8430` |


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
    Reqs/sec      8792.82    4248.64   52040.86
    Latency        5.68ms     4.30ms   370.29ms
    HTTP codes:
      1xx - 0, 2xx - 93524, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6476
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6476
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
    Reqs/sec      7121.47    5028.28   59051.34
    Latency        7.02ms     4.00ms   365.85ms
    HTTP codes:
      1xx - 0, 2xx - 90305, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9695
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9695
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
    Reqs/sec     24328.37    7489.39   35946.31
    Latency        2.05ms     2.06ms   183.07ms
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
    Reqs/sec     21362.18    6105.31   29592.56
    Latency        2.34ms     2.08ms   183.13ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.82MB/s
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
    Reqs/sec     62544.61    3970.59   75201.11
    Latency      797.14us    69.17us     3.08ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.88MB/s
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
    Reqs/sec     18864.43    7658.57   69375.36
    Latency        2.64ms     2.28ms   201.18ms
    HTTP codes:
      1xx - 0, 2xx - 92200, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7800
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7800
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
    Reqs/sec     26298.45    7765.20   55600.29
    Latency        1.90ms     1.93ms   167.80ms
    HTTP codes:
      1xx - 0, 2xx - 97363, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2637
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2637
    Throughput:     5.86MB/s
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
    Reqs/sec     73238.97    5247.80   81761.00
    Latency      680.73us   148.47us     5.61ms
    HTTP codes:
      1xx - 0, 2xx - 98012, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1988
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1988
    Throughput:    11.35MB/s
  ```


