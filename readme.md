## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74239` | `8142` | `85383` |
| **85%** | [Hyper Express](#hyper-express) | `63012` | `3840` | `83053` |
| **35%** | [Node (Default)](#node-default) | `25742` | `7025` | `48839` |
| **34%** | [Fastify](#fastify) | `25491` | `7800` | `36693` |
| **29%** | [Hono](#hono) | `21460` | `6177` | `31970` |
| **26%** | [Koa](#koa) | `19043` | `7054` | `62834` |
| **11%** | [Carbon](#carbon) | `8240` | `1440` | `10413` |
| **9%** | [Express](#express) | `6571` | `1041` | `8600` |


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
    Reqs/sec      8710.27    3826.27   51800.69
    Latency        5.73ms     4.16ms   362.17ms
    HTTP codes:
      1xx - 0, 2xx - 94945, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5055
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5055
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
    Reqs/sec      6602.59    1059.33    8532.06
    Latency        7.57ms     3.53ms   335.72ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.89MB/s
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
    Reqs/sec     24577.18    7345.20   36286.81
    Latency        2.03ms     1.93ms   172.22ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.57MB/s
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
    Reqs/sec     21381.63    5722.42   30814.56
    Latency        2.34ms     1.92ms   173.99ms
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
    Reqs/sec     63061.89    3607.44   66857.33
    Latency      790.61us    66.67us     4.22ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.96MB/s
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
    Reqs/sec     17838.80    6413.25   51681.91
    Latency        2.79ms     2.30ms   200.11ms
    HTTP codes:
      1xx - 0, 2xx - 94393, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5607
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5607
    Throughput:     3.81MB/s
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
    Reqs/sec     26231.18    7570.14   49375.65
    Latency        1.90ms     1.89ms   162.18ms
    HTTP codes:
      1xx - 0, 2xx - 97473, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2527
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2527
    Throughput:     5.86MB/s
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
    Reqs/sec     74089.50    6282.08   81039.08
    Latency      671.06us   217.75us    10.87ms
    HTTP codes:
      1xx - 0, 2xx - 97038, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2962
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2962
    Throughput:    11.37MB/s
  ```


