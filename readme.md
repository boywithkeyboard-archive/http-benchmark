## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `69664` | `4707` | `78040` |
| **83%** | [Hyper Express](#hyper-express) | `57935` | `3247` | `65921` |
| **30%** | [Hono](#hono) | `20587` | `6305` | `29725` |
| **29%** | [Fastify](#fastify) | `20514` | `4970` | `36757` |
| **29%** | [Node (Default)](#node-default) | `20330` | `4905` | `59459` |
| **26%** | [Koa](#koa) | `18246` | `8200` | `72265` |
| **10%** | [Carbon](#carbon) | `7310` | `1289` | `10175` |
| **9%** | [Express](#express) | `6047` | `1075` | `8272` |


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
    Reqs/sec      8031.83    6378.42   76549.85
    Latency        6.22ms     4.60ms   388.15ms
    HTTP codes:
      1xx - 0, 2xx - 89788, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10212
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10212
    Throughput:     1.64MB/s
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
    Reqs/sec      6080.30    1062.71    8145.23
    Latency        8.21ms     3.98ms   375.49ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.74MB/s
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
    Reqs/sec     20552.54    5138.39   36624.30
    Latency        2.43ms     2.10ms   186.34ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.66MB/s
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
    Reqs/sec     20871.01    6659.88   30104.33
    Latency        2.39ms     2.21ms   197.19ms
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
    Reqs/sec     57528.64    3907.88   64545.86
    Latency        0.87ms   164.78us     8.22ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.17MB/s
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
    Reqs/sec     19225.76    9994.71   76994.60
    Latency        2.59ms     2.48ms   217.99ms
    HTTP codes:
      1xx - 0, 2xx - 88998, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11002
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11002
    Throughput:     3.87MB/s
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
    Reqs/sec     19690.45    5831.92   74321.07
    Latency        2.53ms     1.91ms   164.18ms
    HTTP codes:
      1xx - 0, 2xx - 95923, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4077
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4077
    Throughput:     4.33MB/s
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
    Reqs/sec     70213.34    3610.47   80977.27
    Latency      709.25us   202.82us    10.53ms
    HTTP codes:
      1xx - 0, 2xx - 95156, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4844
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4844
    Throughput:    10.57MB/s
  ```


