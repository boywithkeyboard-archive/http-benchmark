## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74683` | `3044` | `93596` |
| **85%** | [Hyper Express](#hyper-express) | `63774` | `3791` | `67552` |
| **31%** | [Node (Default)](#node-default) | `23203` | `6300` | `67633` |
| **28%** | [Fastify](#fastify) | `21235` | `5117` | `35897` |
| **25%** | [Hono](#hono) | `18496` | `4325` | `29402` |
| **22%** | [Koa](#koa) | `16468` | `6794` | `67633` |
| **11%** | [Carbon](#carbon) | `8523` | `1481` | `10769` |
| **9%** | [Express](#express) | `6598` | `1155` | `8524` |


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
    Reqs/sec      9035.65    6728.70   75990.48
    Latency        5.52ms     4.32ms   372.83ms
    HTTP codes:
      1xx - 0, 2xx - 89968, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10032
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10032
    Throughput:     1.85MB/s
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
    Reqs/sec      6410.74    1041.68    8580.25
    Latency        7.80ms     3.67ms   350.78ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     21831.34    5489.60   34883.94
    Latency        2.29ms     1.96ms   174.59ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.95MB/s
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
    Reqs/sec     18874.51    4428.26   29231.70
    Latency        2.65ms     1.95ms   177.29ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.27MB/s
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
    Reqs/sec     63458.72    4063.08   78068.89
    Latency      788.06us    76.00us     3.12ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.99MB/s
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
    Reqs/sec     17012.96    7462.70   76358.18
    Latency        2.93ms     2.29ms   202.22ms
    HTTP codes:
      1xx - 0, 2xx - 92310, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7690
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7690
    Throughput:     3.55MB/s
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
    Reqs/sec     23122.31    6428.76   65436.94
    Latency        2.16ms     1.90ms   161.76ms
    HTTP codes:
      1xx - 0, 2xx - 96965, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3035
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3035
    Throughput:     5.13MB/s
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
    Reqs/sec     75091.11    2250.85   81494.07
    Latency      663.64us   123.32us     5.70ms
    HTTP codes:
      1xx - 0, 2xx - 97132, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2868
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2868
    Throughput:    11.54MB/s
  ```


