## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `79049` | `6516` | `84240` |
| **91%** | [Hyper Express](#hyper-express) | `72033` | `4167` | `76652` |
| **44%** | [Node (Default)](#node-default) | `34415` | `9819` | `90596` |
| **42%** | [Fastify](#fastify) | `33463` | `10815` | `52109` |
| **38%** | [Hono](#hono) | `29969` | `9658` | `50396` |
| **38%** | [Koa](#koa) | `29806` | `13566` | `80480` |
| **11%** | [Carbon](#carbon) | `8917` | `2154` | `13278` |
| **9%** | [Express](#express) | `7188` | `1586` | `10065` |


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
    Reqs/sec     10351.85    7504.89   93669.22
    Latency        4.82ms     4.31ms   372.64ms
    HTTP codes:
      1xx - 0, 2xx - 90985, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9015
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9015
    Throughput:     2.14MB/s
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
    Reqs/sec      8448.18    7924.03   86868.03
    Latency        5.90ms     3.83ms   347.53ms
    HTTP codes:
      1xx - 0, 2xx - 87750, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12250
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12250
    Throughput:     2.13MB/s
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
    Reqs/sec     33036.21    9665.42   53539.16
    Latency        1.51ms     2.01ms   175.24ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.49MB/s
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
    Reqs/sec     29339.90    7791.93   48078.77
    Latency        1.70ms     2.02ms   176.76ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     6.63MB/s
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
    Reqs/sec     71488.37    4119.55   76413.34
    Latency      697.72us    80.33us     2.61ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    10.15MB/s
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
    Reqs/sec     27280.61   13128.00   93609.58
    Latency        1.83ms     2.27ms   198.39ms
    HTTP codes:
      1xx - 0, 2xx - 90513, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9487
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9487
    Throughput:     5.59MB/s
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
    Reqs/sec     33814.95   10395.43   91361.45
    Latency        1.48ms     1.69ms   147.82ms
    HTTP codes:
      1xx - 0, 2xx - 94624, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5376
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5376
    Throughput:     7.31MB/s
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
    Reqs/sec     79502.06    3255.85   86017.69
    Latency      626.53us   179.63us     7.54ms
    HTTP codes:
      1xx - 0, 2xx - 96671, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3329
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3329
    Throughput:    12.17MB/s
  ```


