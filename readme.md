## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74266` | `3118` | `86448` |
| **82%** | [Hyper Express](#hyper-express) | `60957` | `5695` | `79649` |
| **34%** | [Node (Default)](#node-default) | `25547` | `8429` | `73344` |
| **31%** | [Fastify](#fastify) | `22656` | `6651` | `35180` |
| **29%** | [Hono](#hono) | `21167` | `6376` | `30046` |
| **26%** | [Koa](#koa) | `18941` | `9136` | `73354` |
| **11%** | [Carbon](#carbon) | `8079` | `1453` | `10332` |
| **8%** | [Express](#express) | `6178` | `1022` | `8241` |


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
    Reqs/sec      8746.97    6428.38   77892.51
    Latency        5.70ms     4.42ms   380.16ms
    HTTP codes:
      1xx - 0, 2xx - 90389, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9611
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9611
    Throughput:     1.80MB/s
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
    Reqs/sec      6372.98    1147.12    8355.08
    Latency        7.84ms     3.90ms   371.80ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     23972.31    7568.58   35323.59
    Latency        2.08ms     2.09ms   185.85ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.44MB/s
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
    Reqs/sec     20254.66    5890.18   29399.78
    Latency        2.47ms     2.08ms   187.35ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.57MB/s
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
    Reqs/sec     61048.38    3981.61   63755.60
    Latency      817.09us    86.12us     3.93ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.67MB/s
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
    Reqs/sec     19164.71    9636.90   78374.27
    Latency        2.60ms     2.47ms   217.18ms
    HTTP codes:
      1xx - 0, 2xx - 90190, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9810
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9810
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
    Reqs/sec     25748.82    7920.10   61961.41
    Latency        1.94ms     2.00ms   172.07ms
    HTTP codes:
      1xx - 0, 2xx - 97401, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2599
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2599
    Throughput:     5.74MB/s
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
    Reqs/sec     73689.23    2526.56   78756.04
    Latency      675.03us   177.88us    10.73ms
    HTTP codes:
      1xx - 0, 2xx - 96381, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3619
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3619
    Throughput:    11.24MB/s
  ```


