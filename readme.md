## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72645` | `5175` | `93319` |
| **83%** | [Hyper Express](#hyper-express) | `60378` | `4237` | `80422` |
| **29%** | [Hono](#hono) | `21073` | `6233` | `29949` |
| **29%** | [Fastify](#fastify) | `21070` | `5265` | `36966` |
| **28%** | [Node (Default)](#node-default) | `20547` | `5196` | `65370` |
| **23%** | [Koa](#koa) | `16469` | `8750` | `79130` |
| **10%** | [Carbon](#carbon) | `7462` | `1266` | `10757` |
| **8%** | [Express](#express) | `6166` | `1095` | `8235` |


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
    Reqs/sec      8201.24    6935.79   78046.00
    Latency        6.08ms     4.43ms   381.34ms
    HTTP codes:
      1xx - 0, 2xx - 88788, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11212
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11212
    Throughput:     1.65MB/s
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
    Reqs/sec      6185.64    1065.85    8202.22
    Latency        8.08ms     3.75ms   364.98ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.77MB/s
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
    Reqs/sec     20285.47    5464.10   36312.82
    Latency        2.46ms     2.08ms   189.40ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.61MB/s
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
    Reqs/sec     21014.86    6599.69   30757.03
    Latency        2.38ms     2.13ms   192.65ms
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
    Reqs/sec     62064.77    4438.57   74179.12
    Latency      803.40us    90.92us     3.35ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.82MB/s
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
    Reqs/sec     17912.36    8935.51   76629.70
    Latency        2.79ms     2.49ms   216.80ms
    HTTP codes:
      1xx - 0, 2xx - 91715, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8285
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8285
    Throughput:     3.71MB/s
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
    Reqs/sec     19915.31    5445.57   69467.67
    Latency        2.51ms     2.01ms   168.59ms
    HTTP codes:
      1xx - 0, 2xx - 96761, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3239
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3239
    Throughput:     4.40MB/s
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
    Reqs/sec     72440.19    4124.58   83302.58
    Latency      687.79us   156.68us     6.53ms
    HTTP codes:
      1xx - 0, 2xx - 97134, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2866
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2866
    Throughput:    11.14MB/s
  ```


