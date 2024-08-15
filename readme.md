## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73696` | `7277` | `81425` |
| **86%** | [Hyper Express](#hyper-express) | `63600` | `4848` | `92716` |
| **35%** | [Node (Default)](#node-default) | `25504` | `7745` | `57691` |
| **32%** | [Fastify](#fastify) | `23315` | `6512` | `35693` |
| **28%** | [Hono](#hono) | `20416` | `5656` | `29740` |
| **24%** | [Koa](#koa) | `17711` | `6981` | `54342` |
| **11%** | [Carbon](#carbon) | `8133` | `1338` | `10396` |
| **9%** | [Express](#express) | `6412` | `956` | `8432` |


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
    Reqs/sec      8593.72    4474.73   54884.62
    Latency        5.81ms     4.27ms   362.72ms
    HTTP codes:
      1xx - 0, 2xx - 93407, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6593
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6593
    Throughput:     1.82MB/s
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
    Reqs/sec      6412.59     996.57    8380.50
    Latency        7.79ms     3.57ms   340.77ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     22534.57    6088.35   34381.89
    Latency        2.22ms     2.00ms   177.93ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.11MB/s
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
    Reqs/sec     20414.25    5589.34   29784.19
    Latency        2.45ms     1.96ms   175.74ms
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
    Reqs/sec     63487.96    2909.93   72322.26
    Latency      785.18us    65.12us     3.70ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.02MB/s
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
    Reqs/sec     17622.57    6915.54   60435.04
    Latency        2.84ms     2.29ms   202.00ms
    HTTP codes:
      1xx - 0, 2xx - 92847, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7153
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7153
    Throughput:     3.69MB/s
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
    Reqs/sec     25833.88    7837.46   56104.71
    Latency        1.92ms     1.94ms   167.31ms
    HTTP codes:
      1xx - 0, 2xx - 96320, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3680
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3680
    Throughput:     5.72MB/s
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
    Reqs/sec     74841.63    6449.08   86955.90
    Latency      665.55us   184.12us    10.40ms
    HTTP codes:
      1xx - 0, 2xx - 98064, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1936
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1915
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 21
    Throughput:    11.62MB/s
  ```


