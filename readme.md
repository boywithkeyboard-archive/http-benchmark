## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `68535` | `4143` | `80333` |
| **84%** | [Hyper Express](#hyper-express) | `57428` | `3793` | `65725` |
| **35%** | [Node (Default)](#node-default) | `24088` | `9118` | `71041` |
| **34%** | [Fastify](#fastify) | `23350` | `7570` | `33872` |
| **29%** | [Hono](#hono) | `20177` | `6178` | `27804` |
| **25%** | [Koa](#koa) | `17424` | `6751` | `55699` |
| **11%** | [Carbon](#carbon) | `7350` | `1243` | `10066` |
| **9%** | [Express](#express) | `5964` | `994` | `7900` |


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
    Reqs/sec      8017.31    5409.14   63634.79
    Latency        6.23ms     4.86ms   414.48ms
    HTTP codes:
      1xx - 0, 2xx - 90968, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9032
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9032
    Throughput:     1.66MB/s
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
    Reqs/sec      5946.07     972.06    7942.78
    Latency        8.40ms     3.88ms   374.97ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.70MB/s
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
    Reqs/sec     22518.03    6941.18   32476.02
    Latency        2.22ms     2.15ms   191.36ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.10MB/s
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
    Reqs/sec     19157.48    5648.00   27089.33
    Latency        2.61ms     2.37ms   211.29ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.32MB/s
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
    Reqs/sec     57212.75    3675.16   62243.63
    Latency        0.87ms   101.14us     5.68ms
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
    Reqs/sec     18924.92    8281.69   74471.13
    Latency        2.64ms     2.46ms   214.67ms
    HTTP codes:
      1xx - 0, 2xx - 92231, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7769
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7769
    Throughput:     3.95MB/s
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
    Reqs/sec     24953.66    8774.41   69470.99
    Latency        2.00ms     2.11ms   178.63ms
    HTTP codes:
      1xx - 0, 2xx - 95971, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4029
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4029
    Throughput:     5.48MB/s
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
    Reqs/sec     69194.42    3769.96   82430.89
    Latency      718.39us   186.23us     9.04ms
    HTTP codes:
      1xx - 0, 2xx - 95454, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4546
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4546
    Throughput:    10.46MB/s
  ```


