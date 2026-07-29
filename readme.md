## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `78588` | `2817` | `84896` |
| **88%** | [Hyper Express](#hyper-express) | `69159` | `2938` | `74019` |
| **44%** | [Node (Default)](#node-default) | `34539` | `9856` | `73928` |
| **41%** | [Fastify](#fastify) | `32555` | `8747` | `50482` |
| **39%** | [Hono](#hono) | `30684` | `8735` | `46608` |
| **39%** | [Koa](#koa) | `30554` | `13703` | `87304` |
| **12%** | [Carbon](#carbon) | `9393` | `2295` | `13597` |
| **10%** | [Express](#express) | `7737` | `1756` | `10646` |


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
    Reqs/sec     10045.43    6406.37   73538.22
    Latency        4.97ms     4.38ms   372.92ms
    HTTP codes:
      1xx - 0, 2xx - 92309, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7691
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7691
    Throughput:     2.11MB/s
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
    Reqs/sec      8233.87    7321.11   88243.33
    Latency        6.07ms     3.77ms   346.91ms
    HTTP codes:
      1xx - 0, 2xx - 88432, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11568
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11568
    Throughput:     2.08MB/s
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
    Reqs/sec     33134.21    9071.70   51400.99
    Latency        1.51ms     1.88ms   166.75ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.52MB/s
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
    Reqs/sec     31589.56    9827.23   45902.74
    Latency        1.58ms     2.10ms   183.37ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.14MB/s
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
    Reqs/sec     67491.29    4655.78   73108.54
    Latency      739.50us   110.50us     4.34ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.58MB/s
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
    Reqs/sec     30861.26   13817.82   84984.44
    Latency        1.61ms     2.24ms   197.75ms
    HTTP codes:
      1xx - 0, 2xx - 91439, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8561
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8561
    Throughput:     6.39MB/s
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
    Reqs/sec     36717.08   10879.58   74800.81
    Latency        1.36ms     1.84ms   149.94ms
    HTTP codes:
      1xx - 0, 2xx - 96361, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3639
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3639
    Throughput:     8.10MB/s
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
    Reqs/sec     79307.29    3829.46   90113.82
    Latency      628.91us   157.88us     9.31ms
    HTTP codes:
      1xx - 0, 2xx - 96736, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3264
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3264
    Throughput:    12.12MB/s
  ```


