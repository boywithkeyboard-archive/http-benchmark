## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73587` | `2271` | `79409` |
| **83%** | [Hyper Express](#hyper-express) | `60799` | `4657` | `80933` |
| **34%** | [Node (Default)](#node-default) | `25326` | `8579` | `76241` |
| **32%** | [Fastify](#fastify) | `23377` | `8044` | `37901` |
| **28%** | [Hono](#hono) | `20260` | `6358` | `31080` |
| **24%** | [Koa](#koa) | `17853` | `7673` | `64033` |
| **11%** | [Carbon](#carbon) | `7952` | `1442` | `10360` |
| **9%** | [Express](#express) | `6419` | `1143` | `8412` |


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
    Reqs/sec      8519.55    4644.87   61092.09
    Latency        5.86ms     4.26ms   367.77ms
    HTTP codes:
      1xx - 0, 2xx - 93768, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6232
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6232
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
    Reqs/sec      6335.83    1101.52    8478.81
    Latency        7.89ms     3.65ms   351.96ms
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
    Reqs/sec     23313.20    7488.82   34686.85
    Latency        2.14ms     2.29ms   200.26ms
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
    Reqs/sec     21060.13    6470.76   31507.34
    Latency        2.37ms     2.04ms   184.70ms
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
    Reqs/sec     60294.65    3591.48   64249.78
    Latency      826.99us    77.39us     3.03ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.57MB/s
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
    Reqs/sec     19077.76    9462.56   80166.72
    Latency        2.62ms     2.36ms   208.54ms
    HTTP codes:
      1xx - 0, 2xx - 91068, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8932
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8932
    Throughput:     3.93MB/s
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
    Reqs/sec     26017.31    9488.64   65436.10
    Latency        1.92ms     2.05ms   172.59ms
    HTTP codes:
      1xx - 0, 2xx - 97135, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2865
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2865
    Throughput:     5.78MB/s
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
    Reqs/sec     73172.57    2551.63   82889.42
    Latency      680.64us   148.12us     7.74ms
    HTTP codes:
      1xx - 0, 2xx - 97453, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2547
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2547
    Throughput:    11.29MB/s
  ```


