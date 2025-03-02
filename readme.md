## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73159` | `5221` | `78178` |
| **86%** | [Hyper Express](#hyper-express) | `62682` | `3486` | `81304` |
| **36%** | [Node (Default)](#node-default) | `26017` | `7544` | `51664` |
| **33%** | [Fastify](#fastify) | `23959` | `7374` | `35804` |
| **28%** | [Hono](#hono) | `20229` | `5580` | `30318` |
| **26%** | [Koa](#koa) | `19158` | `7159` | `57790` |
| **11%** | [Carbon](#carbon) | `8200` | `1384` | `10454` |
| **9%** | [Express](#express) | `6411` | `1028` | `9360` |


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
    Reqs/sec      8888.21    4669.91   59616.52
    Latency        5.62ms     4.30ms   370.34ms
    HTTP codes:
      1xx - 0, 2xx - 93244, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6756
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6756
    Throughput:     1.88MB/s
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
    Reqs/sec      6313.95    1004.33    8555.15
    Latency        7.92ms     3.80ms   362.79ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.81MB/s
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
    Reqs/sec     24825.32    7617.04   36557.25
    Latency        2.01ms     2.04ms   184.13ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.63MB/s
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
    Reqs/sec     20998.77    5910.68   29738.84
    Latency        2.38ms     2.12ms   188.34ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.75MB/s
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
    Reqs/sec     62148.22    3703.25   65664.00
    Latency      802.46us    73.76us     2.98ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.83MB/s
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
    Reqs/sec     18215.22    7059.00   59599.76
    Latency        2.74ms     2.43ms   209.05ms
    HTTP codes:
      1xx - 0, 2xx - 93188, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6812
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6812
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
    Reqs/sec     26638.77    7440.27   52887.05
    Latency        1.87ms     1.99ms   169.99ms
    HTTP codes:
      1xx - 0, 2xx - 97415, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2585
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2585
    Throughput:     5.94MB/s
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
    Reqs/sec     75948.47    5871.34   85850.51
    Latency      656.01us   137.33us     5.21ms
    HTTP codes:
      1xx - 0, 2xx - 97472, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2528
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2528
    Throughput:    11.72MB/s
  ```


