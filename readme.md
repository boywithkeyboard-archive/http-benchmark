## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73076` | `4170` | `79539` |
| **84%** | [Hyper Express](#hyper-express) | `61107` | `3695` | `64435` |
| **36%** | [Node (Default)](#node-default) | `25956` | `7386` | `55026` |
| **32%** | [Fastify](#fastify) | `23541` | `6905` | `36283` |
| **28%** | [Hono](#hono) | `20593` | `5646` | `29769` |
| **26%** | [Koa](#koa) | `18645` | `6667` | `50574` |
| **11%** | [Carbon](#carbon) | `8273` | `1418` | `10419` |
| **9%** | [Express](#express) | `6625` | `1087` | `8448` |


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
    Reqs/sec      8914.59    4874.90   54513.73
    Latency        5.59ms     4.31ms   374.29ms
    HTTP codes:
      1xx - 0, 2xx - 92227, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7773
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7773
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
    Reqs/sec      7028.16    5097.26   55574.67
    Latency        7.11ms     3.86ms   356.79ms
    HTTP codes:
      1xx - 0, 2xx - 90260, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9740
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9740
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
    Reqs/sec     25595.38    8139.17   35976.84
    Latency        1.95ms     2.02ms   179.32ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.80MB/s
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
    Reqs/sec     20834.61    5742.08   29513.06
    Latency        2.40ms     2.09ms   188.38ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.71MB/s
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
    Reqs/sec     61308.99    3154.24   64008.64
    Latency      813.56us    65.00us     2.76ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.71MB/s
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
    Reqs/sec     19260.65    6169.78   43565.95
    Latency        2.59ms     2.31ms   202.75ms
    HTTP codes:
      1xx - 0, 2xx - 94988, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5012
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5012
    Throughput:     4.14MB/s
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
    Reqs/sec     27309.64    8637.52   51415.83
    Latency        1.83ms     1.93ms   166.86ms
    HTTP codes:
      1xx - 0, 2xx - 97717, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2283
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2283
    Throughput:     6.11MB/s
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
    Reqs/sec     73728.89    4164.47   79117.35
    Latency      676.36us   144.38us     6.10ms
    HTTP codes:
      1xx - 0, 2xx - 97540, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2460
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2460
    Throughput:    11.37MB/s
  ```


