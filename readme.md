## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74799` | `5626` | `86991` |
| **86%** | [Hyper Express](#hyper-express) | `64456` | `4429` | `72605` |
| **36%** | [Node (Default)](#node-default) | `26717` | `7741` | `51307` |
| **34%** | [Fastify](#fastify) | `25329` | `8077` | `38587` |
| **29%** | [Hono](#hono) | `21326` | `6326` | `30800` |
| **26%** | [Koa](#koa) | `19195` | `7331` | `58085` |
| **11%** | [Carbon](#carbon) | `8343` | `1458` | `10430` |
| **9%** | [Express](#express) | `6496` | `1046` | `8356` |


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
    Reqs/sec      8714.09    3982.64   52133.11
    Latency        5.73ms     4.22ms   367.00ms
    HTTP codes:
      1xx - 0, 2xx - 94597, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5403
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5403
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
    Reqs/sec      7060.57    4490.43   55748.33
    Latency        7.07ms     3.93ms   359.60ms
    HTTP codes:
      1xx - 0, 2xx - 92173, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7827
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7827
    Throughput:     1.86MB/s
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
    Reqs/sec     24639.87    7681.61   38575.38
    Latency        2.03ms     2.09ms   187.06ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.58MB/s
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
    Reqs/sec     20663.24    5207.94   30524.88
    Latency        2.42ms     1.99ms   177.72ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.66MB/s
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
    Reqs/sec     64541.02    3181.83   68638.82
    Latency      772.34us    72.50us     3.00ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.17MB/s
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
    Reqs/sec     18925.26    6946.10   52990.35
    Latency        2.64ms     2.23ms   194.09ms
    HTTP codes:
      1xx - 0, 2xx - 93751, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6249
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6249
    Throughput:     4.01MB/s
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
    Reqs/sec     26106.41    6979.26   46327.19
    Latency        1.91ms     2.00ms   169.52ms
    HTTP codes:
      1xx - 0, 2xx - 97979, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2021
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2021
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
    Reqs/sec     74135.84    5609.15   84488.82
    Latency      672.37us   153.60us     6.86ms
    HTTP codes:
      1xx - 0, 2xx - 97839, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2161
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2161
    Throughput:    11.47MB/s
  ```


