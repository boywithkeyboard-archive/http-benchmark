## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `127278` | `3606` | `130754` |
| **91%** | [Hyper Express](#hyper-express) | `115796` | `6566` | `120692` |
| **54%** | [Node (Default)](#node-default) | `68510` | `17656` | `126601` |
| **52%** | [Fastify](#fastify) | `66334` | `17234` | `85664` |
| **50%** | [Koa](#koa) | `64069` | `21150` | `120339` |
| **49%** | [Hono](#hono) | `62933` | `17539` | `82110` |
| **21%** | [Carbon](#carbon) | `26191` | `7768` | `39316` |
| **15%** | [Express](#express) | `18603` | `3779` | `25383` |


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
    Reqs/sec     27443.57   13435.44  114712.20
    Latency        1.81ms     2.92ms   240.83ms
    HTTP codes:
      1xx - 0, 2xx - 91707, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8293
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8293
    Throughput:     5.74MB/s
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
    Reqs/sec     19531.51   11031.84   97394.69
    Latency        2.56ms     2.35ms   207.51ms
    HTTP codes:
      1xx - 0, 2xx - 89099, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10901
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10901
    Throughput:     4.98MB/s
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
    Reqs/sec     73963.24   22461.05  131647.48
    Latency      673.31us     1.06ms    84.05ms
    HTTP codes:
      1xx - 0, 2xx - 81032, 3xx - 0, 4xx - 0, 5xx - 0
      others - 18968
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 18968
    Throughput:    13.60MB/s
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
    Reqs/sec     67728.39   21995.30  127808.29
    Latency      736.02us     1.17ms    95.79ms
    HTTP codes:
      1xx - 0, 2xx - 89595, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10405
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10405
    Throughput:    13.71MB/s
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
    Reqs/sec    115345.97    4603.74  119535.36
    Latency      431.97us   177.43us     7.67ms
    HTTP codes:
      1xx - 0, 2xx - 92619, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7381
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7381
    Throughput:    15.18MB/s
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
    Reqs/sec     62266.76   19860.17  138390.95
    Latency      798.57us     1.16ms    97.66ms
    HTTP codes:
      1xx - 0, 2xx - 91367, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8633
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8633
    Throughput:    12.89MB/s
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
    Reqs/sec     71510.56   17034.80  126658.33
    Latency      697.13us   758.53us    52.72ms
    HTTP codes:
      1xx - 0, 2xx - 95918, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4082
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4082
    Throughput:    15.70MB/s
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
    Reqs/sec    125312.56    7209.80  133289.99
    Latency      397.00us   132.90us     7.23ms
    HTTP codes:
      1xx - 0, 2xx - 96456, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3544
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3544
    Throughput:    19.12MB/s
  ```


