## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74058` | `3517` | `85104` |
| **83%** | [Hyper Express](#hyper-express) | `61713` | `3488` | `66366` |
| **35%** | [Node (Default)](#node-default) | `25859` | `8401` | `62255` |
| **31%** | [Fastify](#fastify) | `23017` | `7119` | `35000` |
| **27%** | [Hono](#hono) | `20013` | `5613` | `29717` |
| **26%** | [Koa](#koa) | `19201` | `8225` | `65647` |
| **11%** | [Carbon](#carbon) | `8319` | `1474` | `10465` |
| **9%** | [Express](#express) | `6369` | `1063` | `8324` |


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
    Reqs/sec      8907.78    5738.90   68881.21
    Latency        5.60ms     4.39ms   376.85ms
    HTTP codes:
      1xx - 0, 2xx - 92201, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7799
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7799
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
    Reqs/sec      6453.39    1089.96    8353.25
    Latency        7.74ms     3.67ms   352.52ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.85MB/s
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
    Reqs/sec     24390.45    7867.43   37543.33
    Latency        2.05ms     2.11ms   189.01ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.53MB/s
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
    Reqs/sec     20649.02    5836.56   30086.68
    Latency        2.42ms     1.90ms   173.18ms
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
    Reqs/sec     62156.04    3092.03   65106.43
    Latency      802.63us    72.85us     2.82ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.83MB/s
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
    Reqs/sec     19060.08    9101.93   79854.54
    Latency        2.62ms     2.42ms   214.41ms
    HTTP codes:
      1xx - 0, 2xx - 91844, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8156
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8156
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
    Reqs/sec     25899.68    8834.22   67556.20
    Latency        1.92ms     1.96ms   167.17ms
    HTTP codes:
      1xx - 0, 2xx - 97059, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2941
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2941
    Throughput:     5.77MB/s
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
    Reqs/sec     74971.78    2552.69   86652.81
    Latency      663.26us   147.79us     7.55ms
    HTTP codes:
      1xx - 0, 2xx - 96373, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3627
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3627
    Throughput:    11.44MB/s
  ```


