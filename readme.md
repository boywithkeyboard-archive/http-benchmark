## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72020` | `4561` | `85589` |
| **81%** | [Hyper Express](#hyper-express) | `58398` | `5098` | `74874` |
| **29%** | [Fastify](#fastify) | `20950` | `5872` | `37770` |
| **29%** | [Hono](#hono) | `20678` | `5994` | `31185` |
| **28%** | [Node (Default)](#node-default) | `20188` | `5651` | `73375` |
| **27%** | [Koa](#koa) | `19086` | `9456` | `79537` |
| **10%** | [Carbon](#carbon) | `7386` | `1287` | `10242` |
| **9%** | [Express](#express) | `6131` | `1062` | `8184` |


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
    Reqs/sec      7742.68    5753.41   75125.67
    Latency        6.45ms     4.52ms   396.94ms
    HTTP codes:
      1xx - 0, 2xx - 91553, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8447
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8447
    Throughput:     1.61MB/s
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
    Reqs/sec      5952.99    1032.49    8206.49
    Latency        8.40ms     3.81ms   368.31ms
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
    Reqs/sec     21100.18    6259.44   37005.73
    Latency        2.37ms     2.10ms   189.52ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.79MB/s
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
    Reqs/sec     21491.56    6725.53   30852.70
    Latency        2.33ms     2.15ms   189.65ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.85MB/s
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
    Reqs/sec     61243.95    3667.11   69205.13
    Latency      814.26us    89.10us     3.70ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.70MB/s
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
    Reqs/sec     19007.88    8641.68   63362.22
    Latency        2.62ms     2.48ms   214.57ms
    HTTP codes:
      1xx - 0, 2xx - 93436, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6564
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6564
    Throughput:     4.02MB/s
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
    Reqs/sec     20623.37    5965.41   66282.96
    Latency        2.42ms     2.04ms   174.51ms
    HTTP codes:
      1xx - 0, 2xx - 96235, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3765
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3765
    Throughput:     4.55MB/s
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
    Reqs/sec     71807.02    3379.10   81170.60
    Latency      692.43us   248.91us    13.39ms
    HTTP codes:
      1xx - 0, 2xx - 95138, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4862
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4862
    Throughput:    10.82MB/s
  ```


