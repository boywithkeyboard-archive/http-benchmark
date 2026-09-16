## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `101448` | `4990` | `110754` |
| **88%** | [Hyper Express](#hyper-express) | `89057` | `3803` | `93167` |
| **47%** | [Node (Default)](#node-default) | `47868` | `13466` | `83595` |
| **42%** | [Fastify](#fastify) | `42651` | `12753` | `66569` |
| **39%** | [Hono](#hono) | `39101` | `12899` | `59886` |
| **38%** | [Koa](#koa) | `38375` | `15370` | `97540` |
| **12%** | [Carbon](#carbon) | `12029` | `2766` | `17304` |
| **9%** | [Express](#express) | `9283` | `1953` | `13592` |


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
    Reqs/sec     12813.79    9218.72   96878.49
    Latency        3.90ms     3.37ms   288.34ms
    HTTP codes:
      1xx - 0, 2xx - 89481, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10519
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10519
    Throughput:     2.60MB/s
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
    Reqs/sec     10335.16    8674.66   92315.10
    Latency        4.83ms     2.95ms   269.88ms
    HTTP codes:
      1xx - 0, 2xx - 89629, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10371
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10371
    Throughput:     2.65MB/s
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
    Reqs/sec     45804.04   14504.95   66678.19
    Latency        1.09ms     1.43ms   126.37ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    10.39MB/s
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
    Reqs/sec     39151.21   12255.70   59889.75
    Latency        1.28ms     1.58ms   136.67ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.82MB/s
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
    Reqs/sec     86954.86    4630.27   92400.45
    Latency      573.70us    82.81us     3.10ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    12.35MB/s
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
    Reqs/sec     39077.32   15608.86   94413.28
    Latency        1.27ms     1.77ms   154.65ms
    HTTP codes:
      1xx - 0, 2xx - 92005, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7995
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7995
    Throughput:     8.15MB/s
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
    Reqs/sec     47627.48   13752.27  103620.19
    Latency        1.05ms     1.28ms   105.69ms
    HTTP codes:
      1xx - 0, 2xx - 95897, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4103
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4103
    Throughput:    10.46MB/s
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
    Reqs/sec    102620.99    4216.61  117858.74
    Latency      485.85us   159.03us     8.21ms
    HTTP codes:
      1xx - 0, 2xx - 96558, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3442
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3442
    Throughput:    15.68MB/s
  ```


