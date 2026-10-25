## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `69686` | `3573` | `79861` |
| **80%** | [Hyper Express](#hyper-express) | `55616` | `4809` | `63336` |
| **30%** | [Fastify](#fastify) | `20869` | `5741` | `35750` |
| **29%** | [Hono](#hono) | `20423` | `6268` | `31179` |
| **29%** | [Node (Default)](#node-default) | `20331` | `7116` | `83977` |
| **28%** | [Koa](#koa) | `19299` | `8002` | `64377` |
| **10%** | [Carbon](#carbon) | `7267` | `1237` | `10203` |
| **9%** | [Express](#express) | `5954` | `1070` | `8187` |


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
    Reqs/sec      7937.63    6303.68   66344.85
    Latency        6.28ms     4.64ms   396.74ms
    HTTP codes:
      1xx - 0, 2xx - 89058, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10942
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10942
    Throughput:     1.61MB/s
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
    Reqs/sec      6123.62    1140.56    8177.46
    Latency        8.16ms     4.06ms   385.59ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.75MB/s
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
    Reqs/sec     22331.53    7622.20   36180.80
    Latency        2.24ms     2.13ms   193.93ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.06MB/s
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
    Reqs/sec     20697.16    6288.09   29431.45
    Latency        2.42ms     2.17ms   194.95ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.67MB/s
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
    Reqs/sec     57238.99    4638.57   64097.77
    Latency        0.87ms   127.39us     4.89ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.13MB/s
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
    Reqs/sec     19322.84    8640.99   73187.50
    Latency        2.58ms     2.54ms   225.27ms
    HTTP codes:
      1xx - 0, 2xx - 91671, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8329
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8329
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
    Reqs/sec     19685.19    4494.19   56263.55
    Latency        2.53ms     2.01ms   175.82ms
    HTTP codes:
      1xx - 0, 2xx - 97473, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2527
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2527
    Throughput:     4.39MB/s
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
    Reqs/sec     70094.37    4520.79   83648.37
    Latency      709.92us   199.83us    10.97ms
    HTTP codes:
      1xx - 0, 2xx - 95649, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4351
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4351
    Throughput:    10.61MB/s
  ```


