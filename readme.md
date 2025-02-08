## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73276` | `4900` | `80912` |
| **86%** | [Hyper Express](#hyper-express) | `62712` | `3756` | `66751` |
| **37%** | [Node (Default)](#node-default) | `27117` | `7718` | `50480` |
| **32%** | [Fastify](#fastify) | `23651` | `6858` | `36552` |
| **29%** | [Hono](#hono) | `20977` | `5721` | `30615` |
| **26%** | [Koa](#koa) | `18835` | `7038` | `62017` |
| **11%** | [Carbon](#carbon) | `8282` | `1437` | `10392` |
| **9%** | [Express](#express) | `6509` | `1071` | `8483` |


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
    Reqs/sec      8793.33    4427.39   56424.96
    Latency        5.68ms     4.15ms   361.25ms
    HTTP codes:
      1xx - 0, 2xx - 94059, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5941
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5941
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
    Reqs/sec      7004.22    4226.60   48046.58
    Latency        7.13ms     3.95ms   359.96ms
    HTTP codes:
      1xx - 0, 2xx - 91794, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8206
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8206
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
    Reqs/sec     25583.44    7782.31   38282.55
    Latency        1.95ms     1.86ms   170.70ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.81MB/s
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
    Reqs/sec     21337.50    5807.94   30999.04
    Latency        2.34ms     1.97ms   176.31ms
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
    Reqs/sec     63650.65    3652.76   69984.53
    Latency      783.40us    70.06us     3.12ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.04MB/s
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
    Reqs/sec     18537.46    6733.23   52971.15
    Latency        2.69ms     2.34ms   204.68ms
    HTTP codes:
      1xx - 0, 2xx - 93150, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6850
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6850
    Throughput:     3.91MB/s
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
    Reqs/sec     27485.84    8408.47   60434.50
    Latency        1.82ms     1.89ms   163.93ms
    HTTP codes:
      1xx - 0, 2xx - 96679, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3321
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3321
    Throughput:     6.08MB/s
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
    Reqs/sec     75745.51    6612.05   87957.21
    Latency      658.04us   181.02us     8.53ms
    HTTP codes:
      1xx - 0, 2xx - 98130, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1870
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1870
    Throughput:    11.76MB/s
  ```


