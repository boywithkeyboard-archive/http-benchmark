## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74907` | `2626` | `82189` |
| **84%** | [Hyper Express](#hyper-express) | `63093` | `3421` | `67825` |
| **34%** | [Node (Default)](#node-default) | `25396` | `7898` | `74053` |
| **32%** | [Fastify](#fastify) | `24289` | `7910` | `36305` |
| **27%** | [Hono](#hono) | `20595` | `5903` | `30959` |
| **26%** | [Koa](#koa) | `19551` | `8832` | `67479` |
| **11%** | [Carbon](#carbon) | `8320` | `1518` | `10566` |
| **8%** | [Express](#express) | `6298` | `1047` | `8441` |


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
    Reqs/sec      8827.54    6613.32   80204.56
    Latency        5.65ms     4.38ms   378.69ms
    HTTP codes:
      1xx - 0, 2xx - 90715, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9285
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9285
    Throughput:     1.82MB/s
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
    Reqs/sec      6984.58    5509.80   67435.63
    Latency        7.15ms     3.95ms   364.24ms
    HTTP codes:
      1xx - 0, 2xx - 91020, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8980
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8980
    Throughput:     1.82MB/s
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
    Reqs/sec     23886.74    7368.67   35529.32
    Latency        2.09ms     2.10ms   189.82ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.41MB/s
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
    Reqs/sec     21121.68    5736.74   30030.55
    Latency        2.36ms     1.97ms   178.17ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.77MB/s
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
    Reqs/sec     63106.46    3762.35   67133.72
    Latency      790.72us    68.24us     2.87ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.96MB/s
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
    Reqs/sec     18377.62    7924.99   68177.75
    Latency        2.72ms     2.37ms   208.13ms
    HTTP codes:
      1xx - 0, 2xx - 93161, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6839
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6839
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
    Reqs/sec     26041.40    8246.32   65991.67
    Latency        1.92ms     1.82ms   157.61ms
    HTTP codes:
      1xx - 0, 2xx - 97322, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2678
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2678
    Throughput:     5.79MB/s
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
    Reqs/sec     74202.39    2222.60   81621.11
    Latency      671.48us   153.97us     7.88ms
    HTTP codes:
      1xx - 0, 2xx - 97071, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2929
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2929
    Throughput:    11.40MB/s
  ```


