## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75944` | `5896` | `91324` |
| **85%** | [Hyper Express](#hyper-express) | `64476` | `4933` | `82050` |
| **35%** | [Node (Default)](#node-default) | `26528` | `7685` | `53070` |
| **31%** | [Fastify](#fastify) | `23272` | `6626` | `36294` |
| **27%** | [Hono](#hono) | `20788` | `5617` | `29979` |
| **24%** | [Koa](#koa) | `18262` | `6054` | `54014` |
| **11%** | [Carbon](#carbon) | `8326` | `1408` | `10453` |
| **9%** | [Express](#express) | `6589` | `1052` | `8609` |


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
    Reqs/sec      8778.30    4149.10   50310.64
    Latency        5.69ms     4.21ms   363.36ms
    HTTP codes:
      1xx - 0, 2xx - 94398, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5602
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5602
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
    Reqs/sec      7179.79    4471.95   57769.23
    Latency        6.95ms     3.78ms   345.31ms
    HTTP codes:
      1xx - 0, 2xx - 91831, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8169
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8161
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 8
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
    Reqs/sec     24273.85    7079.82   35321.81
    Latency        2.06ms     1.86ms   170.28ms
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
    Reqs/sec     20795.71    5561.36   29933.80
    Latency        2.40ms     1.82ms   167.92ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.70MB/s
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
    Reqs/sec     62922.28    2656.40   65560.04
    Latency      791.64us    64.28us     5.39ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.94MB/s
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
    Reqs/sec     18094.82    5937.69   52324.63
    Latency        2.76ms     2.27ms   200.62ms
    HTTP codes:
      1xx - 0, 2xx - 94983, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5017
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5017
    Throughput:     3.89MB/s
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
    Reqs/sec     25379.09    6879.78   51866.64
    Latency        1.97ms     1.90ms   161.38ms
    HTTP codes:
      1xx - 0, 2xx - 97694, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2306
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2306
    Throughput:     5.68MB/s
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
    Reqs/sec     75188.99    5545.34   86295.11
    Latency      662.75us   214.99us     9.86ms
    HTTP codes:
      1xx - 0, 2xx - 96642, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3358
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3358
    Throughput:    11.48MB/s
  ```


