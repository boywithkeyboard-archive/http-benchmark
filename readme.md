## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74185` | `2780` | `79880` |
| **83%** | [Hyper Express](#hyper-express) | `61247` | `3818` | `64306` |
| **37%** | [Node (Default)](#node-default) | `27297` | `8876` | `67560` |
| **32%** | [Fastify](#fastify) | `23865` | `7298` | `36890` |
| **28%** | [Hono](#hono) | `20488` | `5873` | `30411` |
| **26%** | [Koa](#koa) | `19422` | `9306` | `79496` |
| **11%** | [Carbon](#carbon) | `8206` | `1455` | `10336` |
| **9%** | [Express](#express) | `6342` | `1059` | `8329` |


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
    Reqs/sec      9001.25    6519.56   81827.11
    Latency        5.54ms     4.27ms   368.30ms
    HTTP codes:
      1xx - 0, 2xx - 90765, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9235
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9235
    Throughput:     1.86MB/s
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
    Reqs/sec      7124.93    6125.53   78718.42
    Latency        7.01ms     3.83ms   350.25ms
    HTTP codes:
      1xx - 0, 2xx - 89692, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10308
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10308
    Throughput:     1.83MB/s
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
    Reqs/sec     24112.50    7277.23   35816.34
    Latency        2.07ms     1.90ms   173.61ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.47MB/s
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
    Reqs/sec     20546.15    5822.91   30895.64
    Latency        2.43ms     2.07ms   184.50ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.65MB/s
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
    Reqs/sec     61964.02    3273.58   65955.39
    Latency      804.95us    64.02us     2.70ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.80MB/s
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
    Reqs/sec     18262.58    7451.20   62242.08
    Latency        2.73ms     2.45ms   216.35ms
    HTTP codes:
      1xx - 0, 2xx - 93728, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6272
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6272
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
    Reqs/sec     25368.11    7527.55   61423.18
    Latency        1.97ms     1.96ms   168.11ms
    HTTP codes:
      1xx - 0, 2xx - 97274, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2726
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2726
    Throughput:     5.64MB/s
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
    Reqs/sec     74738.35    2930.37   82252.69
    Latency      666.00us   156.07us     8.55ms
    HTTP codes:
      1xx - 0, 2xx - 96531, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3469
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3469
    Throughput:    11.42MB/s
  ```


