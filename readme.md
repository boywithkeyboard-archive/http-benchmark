## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74129` | `5902` | `78241` |
| **85%** | [Hyper Express](#hyper-express) | `63349` | `4201` | `77525` |
| **36%** | [Node (Default)](#node-default) | `26914` | `8462` | `52927` |
| **33%** | [Fastify](#fastify) | `24549` | `7207` | `36158` |
| **27%** | [Hono](#hono) | `20006` | `5208` | `29694` |
| **25%** | [Koa](#koa) | `18276` | `7454` | `61145` |
| **11%** | [Carbon](#carbon) | `8180` | `1342` | `10598` |
| **9%** | [Express](#express) | `6546` | `1016` | `8637` |


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
    Reqs/sec      8777.88    4060.73   56712.96
    Latency        5.69ms     4.20ms   369.83ms
    HTTP codes:
      1xx - 0, 2xx - 94283, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5717
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5717
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
    Reqs/sec      7052.19    3898.13   46377.43
    Latency        7.08ms     3.74ms   340.58ms
    HTTP codes:
      1xx - 0, 2xx - 93091, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6909
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6907
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 2
    Throughput:     1.88MB/s
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
    Reqs/sec     24295.52    7027.65   36310.62
    Latency        2.06ms     1.95ms   176.54ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.51MB/s
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
    Reqs/sec     21100.69    5946.46   29977.74
    Latency        2.37ms     2.02ms   184.41ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.77MB/s
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
    Reqs/sec     63600.40    3932.50   67725.78
    Latency      783.80us    70.15us     3.08ms
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
    Reqs/sec     18232.26    5874.71   45720.25
    Latency        2.74ms     2.32ms   202.93ms
    HTTP codes:
      1xx - 0, 2xx - 95255, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4745
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4745
    Throughput:     3.92MB/s
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
    Reqs/sec     26866.26    8041.24   55768.91
    Latency        1.86ms     1.88ms   162.75ms
    HTTP codes:
      1xx - 0, 2xx - 97251, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2749
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2749
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
    Reqs/sec     74517.82    5905.67   83211.13
    Latency      670.07us   198.50us    20.39ms
    HTTP codes:
      1xx - 0, 2xx - 97783, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2217
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2217
    Throughput:    11.50MB/s
  ```


