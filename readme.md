## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73826` | `6253` | `91645` |
| **86%** | [Hyper Express](#hyper-express) | `63216` | `4019` | `69029` |
| **37%** | [Node (Default)](#node-default) | `27095` | `7809` | `52685` |
| **34%** | [Fastify](#fastify) | `25230` | `7604` | `37303` |
| **28%** | [Hono](#hono) | `20961` | `5645` | `30065` |
| **24%** | [Koa](#koa) | `17862` | `5491` | `52275` |
| **11%** | [Carbon](#carbon) | `8392` | `1432` | `10277` |
| **9%** | [Express](#express) | `6317` | `943` | `8354` |


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
    Reqs/sec      8581.18    3447.35   51673.76
    Latency        5.82ms     4.29ms   373.02ms
    HTTP codes:
      1xx - 0, 2xx - 95452, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4548
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4548
    Throughput:     1.86MB/s
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
    Reqs/sec      6640.74    1109.10    8457.15
    Latency        7.53ms     3.68ms   349.43ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     24401.18    7632.01   37020.24
    Latency        2.05ms     2.00ms   181.20ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.54MB/s
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
    Reqs/sec     20414.10    5356.59   29343.33
    Latency        2.45ms     2.00ms   181.29ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.61MB/s
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
    Reqs/sec     62674.01    3056.21   64933.12
    Latency      795.43us    61.15us     2.77ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.90MB/s
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
    Reqs/sec     19374.96    6846.02   56452.81
    Latency        2.57ms     2.33ms   200.43ms
    HTTP codes:
      1xx - 0, 2xx - 93437, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6563
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6563
    Throughput:     4.10MB/s
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
    Reqs/sec     27070.91    8272.32   53611.15
    Latency        1.84ms     2.06ms   177.02ms
    HTTP codes:
      1xx - 0, 2xx - 97627, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2373
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2373
    Throughput:     6.05MB/s
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
    Reqs/sec     75510.02    6463.89  107522.50
    Latency      663.13us   140.06us     5.81ms
    HTTP codes:
      1xx - 0, 2xx - 97858, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2142
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2142
    Throughput:    11.64MB/s
  ```


