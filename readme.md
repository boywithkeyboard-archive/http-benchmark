## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73316` | `5603` | `78207` |
| **87%** | [Hyper Express](#hyper-express) | `63569` | `4505` | `93518` |
| **34%** | [Node (Default)](#node-default) | `25165` | `7381` | `54068` |
| **33%** | [Fastify](#fastify) | `24039` | `7120` | `35147` |
| **29%** | [Hono](#hono) | `21533` | `5516` | `29311` |
| **25%** | [Koa](#koa) | `18591` | `6230` | `45578` |
| **11%** | [Carbon](#carbon) | `8230` | `1398` | `10435` |
| **9%** | [Express](#express) | `6348` | `963` | `8332` |


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
    Reqs/sec      8761.68    4276.16   53822.20
    Latency        5.69ms     4.18ms   360.92ms
    HTTP codes:
      1xx - 0, 2xx - 93646, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6354
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6354
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
    Reqs/sec      6823.10    3679.89   47286.88
    Latency        7.29ms     3.84ms   357.69ms
    HTTP codes:
      1xx - 0, 2xx - 93021, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6979
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6970
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 9
    Throughput:     1.82MB/s
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
    Reqs/sec     24194.01    7344.39   36479.71
    Latency        2.06ms     1.95ms   178.22ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.49MB/s
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
    Reqs/sec     20411.11    5593.56   30321.81
    Latency        2.45ms     1.89ms   172.45ms
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
    Reqs/sec     61963.50    3728.62   67246.20
    Latency      804.61us    65.31us     3.06ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.80MB/s
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
    Reqs/sec     18232.52    6235.52   48681.20
    Latency        2.74ms     2.25ms   196.13ms
    HTTP codes:
      1xx - 0, 2xx - 94570, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5430
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5430
    Throughput:     3.90MB/s
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
    Reqs/sec     26709.13    8338.59   57945.35
    Latency        1.87ms     1.94ms   169.53ms
    HTTP codes:
      1xx - 0, 2xx - 97621, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2379
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2379
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
    Reqs/sec     73479.51    5359.29   78308.48
    Latency      677.85us   202.26us    10.00ms
    HTTP codes:
      1xx - 0, 2xx - 96937, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3063
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3063
    Throughput:    11.27MB/s
  ```


