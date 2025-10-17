## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75739` | `3295` | `80397` |
| **81%** | [Hyper Express](#hyper-express) | `61407` | `3608` | `64296` |
| **32%** | [Node (Default)](#node-default) | `24014` | `6021` | `53149` |
| **30%** | [Fastify](#fastify) | `22870` | `6971` | `35346` |
| **27%** | [Hono](#hono) | `20419` | `5949` | `29411` |
| **24%** | [Koa](#koa) | `18434` | `8554` | `77512` |
| **11%** | [Carbon](#carbon) | `8196` | `1530` | `10322` |
| **8%** | [Express](#express) | `6099` | `994` | `8228` |


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
    Reqs/sec      8737.39    6626.52   70974.18
    Latency        5.71ms     4.40ms   381.00ms
    HTTP codes:
      1xx - 0, 2xx - 89698, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10302
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10302
    Throughput:     1.78MB/s
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
    Reqs/sec      6167.43    1008.28    8158.29
    Latency        8.10ms     3.78ms   364.81ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.76MB/s
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
    Reqs/sec     23340.05    7129.85   36491.27
    Latency        2.14ms     2.13ms   191.64ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.29MB/s
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
    Reqs/sec     20369.95    5988.07   29644.10
    Latency        2.45ms     2.12ms   191.15ms
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
    Reqs/sec     61773.54    3472.22   63968.26
    Latency      807.22us    71.77us     4.06ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.77MB/s
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
    Reqs/sec     19422.68    8482.95   72308.82
    Latency        2.57ms     2.48ms   216.91ms
    HTTP codes:
      1xx - 0, 2xx - 91988, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8012
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8012
    Throughput:     4.04MB/s
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
    Reqs/sec     25844.02    8232.50   75702.42
    Latency        1.93ms     1.95ms   170.18ms
    HTTP codes:
      1xx - 0, 2xx - 96324, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3676
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3676
    Throughput:     5.71MB/s
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
    Reqs/sec     75588.97    3319.31   83440.84
    Latency      658.68us   185.19us     9.97ms
    HTTP codes:
      1xx - 0, 2xx - 96366, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3634
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3634
    Throughput:    11.53MB/s
  ```


