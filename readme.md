## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73603` | `4591` | `86642` |
| **86%** | [Hyper Express](#hyper-express) | `63315` | `3931` | `72566` |
| **36%** | [Node (Default)](#node-default) | `26630` | `7800` | `52860` |
| **32%** | [Fastify](#fastify) | `23422` | `6427` | `36310` |
| **28%** | [Hono](#hono) | `20501` | `5211` | `29760` |
| **26%** | [Koa](#koa) | `19039` | `6540` | `52377` |
| **11%** | [Carbon](#carbon) | `8348` | `1464` | `10299` |
| **9%** | [Express](#express) | `6387` | `999` | `8337` |


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
    Reqs/sec      9126.32    5877.16   64008.17
    Latency        5.47ms     4.35ms   372.99ms
    HTTP codes:
      1xx - 0, 2xx - 90482, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9518
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9518
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
    Reqs/sec      6959.18    4324.50   59090.16
    Latency        7.17ms     3.99ms   361.88ms
    HTTP codes:
      1xx - 0, 2xx - 92363, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7637
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7637
    Throughput:     1.84MB/s
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
    Reqs/sec     24793.98    7610.85   36975.53
    Latency        2.02ms     1.96ms   179.75ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.62MB/s
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
    Reqs/sec     21399.72    6129.46   30330.62
    Latency        2.33ms     2.05ms   181.84ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.84MB/s
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
    Reqs/sec     61761.51    3951.65   71563.93
    Latency      808.87us    89.26us     4.07ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.75MB/s
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
    Reqs/sec     19216.84    6560.04   50249.43
    Latency        2.59ms     2.32ms   203.71ms
    HTTP codes:
      1xx - 0, 2xx - 94104, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5896
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5896
    Throughput:     4.10MB/s
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
    Reqs/sec     27361.39    8642.71   48366.51
    Latency        1.82ms     1.86ms   158.19ms
    HTTP codes:
      1xx - 0, 2xx - 97887, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2113
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2113
    Throughput:     6.13MB/s
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
    Reqs/sec     74640.60    4822.60   82030.76
    Latency      667.10us   181.89us     9.07ms
    HTTP codes:
      1xx - 0, 2xx - 96747, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3253
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3253
    Throughput:    11.43MB/s
  ```


