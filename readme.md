## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74897` | `3931` | `91462` |
| **82%** | [Hyper Express](#hyper-express) | `61588` | `3112` | `64359` |
| **34%** | [Node (Default)](#node-default) | `25375` | `7343` | `61510` |
| **31%** | [Fastify](#fastify) | `23260` | `6963` | `34982` |
| **29%** | [Hono](#hono) | `21487` | `6351` | `30024` |
| **25%** | [Koa](#koa) | `18881` | `7721` | `65232` |
| **11%** | [Carbon](#carbon) | `8148` | `1381` | `10224` |
| **9%** | [Express](#express) | `6370` | `1029` | `8424` |


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
    Reqs/sec      9152.08    6772.45   79370.14
    Latency        5.45ms     4.29ms   368.22ms
    HTTP codes:
      1xx - 0, 2xx - 89967, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10033
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10033
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
    Reqs/sec      7255.81    5996.75   74562.19
    Latency        6.88ms     3.97ms   364.98ms
    HTTP codes:
      1xx - 0, 2xx - 89959, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10041
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10041
    Throughput:     1.87MB/s
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
    Reqs/sec     23614.01    6847.30   35851.60
    Latency        2.12ms     1.94ms   177.72ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.35MB/s
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
    Reqs/sec     20580.54    5786.06   30220.30
    Latency        2.43ms     2.07ms   186.68ms
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
    Reqs/sec     62002.58    3620.93   64581.07
    Latency      804.12us    72.42us     2.95ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.81MB/s
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
    Reqs/sec     19268.07    8123.30   65929.11
    Latency        2.59ms     2.37ms   208.99ms
    HTTP codes:
      1xx - 0, 2xx - 93178, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6822
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6822
    Throughput:     4.07MB/s
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
    Reqs/sec     25454.09    7583.07   57446.39
    Latency        1.96ms     2.03ms   173.80ms
    HTTP codes:
      1xx - 0, 2xx - 97496, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2504
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2504
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
    Reqs/sec     74965.04    4086.03   98726.43
    Latency      666.34us   170.66us    11.21ms
    HTTP codes:
      1xx - 0, 2xx - 96486, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3514
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3514
    Throughput:    11.40MB/s
  ```


