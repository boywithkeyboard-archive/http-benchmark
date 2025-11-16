## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74202` | `2655` | `82340` |
| **84%** | [Hyper Express](#hyper-express) | `62692` | `2876` | `65139` |
| **35%** | [Node (Default)](#node-default) | `25788` | `7414` | `59394` |
| **33%** | [Fastify](#fastify) | `24764` | `7728` | `36615` |
| **28%** | [Hono](#hono) | `20853` | `5514` | `30268` |
| **26%** | [Koa](#koa) | `19252` | `8686` | `81242` |
| **11%** | [Carbon](#carbon) | `8299` | `1472` | `10521` |
| **9%** | [Express](#express) | `6446` | `1075` | `8405` |


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
    Reqs/sec      9053.00    5818.01   78917.35
    Latency        5.51ms     4.24ms   366.46ms
    HTTP codes:
      1xx - 0, 2xx - 92112, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7888
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7888
    Throughput:     1.89MB/s
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
    Reqs/sec      7159.88    6344.10   76593.77
    Latency        6.97ms     3.91ms   359.92ms
    HTTP codes:
      1xx - 0, 2xx - 89313, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10687
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10687
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
    Reqs/sec     24294.61    7276.37   37713.89
    Latency        2.06ms     2.09ms   184.93ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.51MB/s
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
    Reqs/sec     21659.04    6307.86   31474.37
    Latency        2.31ms     1.99ms   178.27ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.89MB/s
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
    Reqs/sec     63454.37    3371.36   67871.04
    Latency      785.79us    69.84us     3.23ms
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
    Reqs/sec     18379.40    8250.38   72611.95
    Latency        2.71ms     2.44ms   215.17ms
    HTTP codes:
      1xx - 0, 2xx - 92399, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7601
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7601
    Throughput:     3.84MB/s
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
    Reqs/sec     26671.14    7951.55   57176.52
    Latency        1.87ms     1.84ms   161.50ms
    HTTP codes:
      1xx - 0, 2xx - 97663, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2337
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2337
    Throughput:     5.97MB/s
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
    Reqs/sec     74212.55    2627.60   82128.87
    Latency      671.25us   128.30us     6.02ms
    HTTP codes:
      1xx - 0, 2xx - 97462, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2538
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2538
    Throughput:    11.45MB/s
  ```


