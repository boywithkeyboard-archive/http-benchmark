## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `118947` | `7133` | `126951` |
| **82%** | [Hyper Express](#hyper-express) | `97296` | `6892` | `103994` |
| **31%** | [Node (Default)](#node-default) | `36753` | `11107` | `108867` |
| **28%** | [Fastify](#fastify) | `33726` | `7121` | `42290` |
| **26%** | [Koa](#koa) | `31070` | `14899` | `109540` |
| **26%** | [Hono](#hono) | `30588` | `6611` | `46498` |
| **10%** | [Carbon](#carbon) | `11794` | `2333` | `16114` |
| **7%** | [Express](#express) | `8394` | `1679` | `11394` |


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
    Reqs/sec     12761.72    9407.88  110472.12
    Latency        3.91ms     3.72ms   322.84ms
    HTTP codes:
      1xx - 0, 2xx - 90094, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9906
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9906
    Throughput:     2.61MB/s
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
    Reqs/sec      9684.83    9642.46  102679.44
    Latency        5.15ms     3.41ms   311.53ms
    HTTP codes:
      1xx - 0, 2xx - 87260, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12740
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12740
    Throughput:     2.42MB/s
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
    Reqs/sec     39183.18   20748.44  113939.60
    Latency        1.27ms     1.66ms   138.67ms
    HTTP codes:
      1xx - 0, 2xx - 75190, 3xx - 0, 4xx - 0, 5xx - 0
      others - 24810
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 24810
    Throughput:     6.69MB/s
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
    Reqs/sec     32342.21   12521.21  100190.76
    Latency        1.54ms     1.68ms   144.30ms
    HTTP codes:
      1xx - 0, 2xx - 90854, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9146
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9146
    Throughput:     6.65MB/s
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
    Reqs/sec     93569.16   11396.24  107109.90
    Latency      530.77us   401.31us    19.55ms
    HTTP codes:
      1xx - 0, 2xx - 92388, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7612
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7612
    Throughput:    12.28MB/s
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
    Reqs/sec     30662.37   13947.81  106887.17
    Latency        1.63ms     1.83ms   155.53ms
    HTTP codes:
      1xx - 0, 2xx - 89415, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10585
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10585
    Throughput:     6.19MB/s
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
    Reqs/sec     37831.06   11513.77  113759.48
    Latency        1.32ms     1.38ms   112.28ms
    HTTP codes:
      1xx - 0, 2xx - 94422, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5578
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5578
    Throughput:     8.19MB/s
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
    Reqs/sec    118670.19    6726.14  134054.33
    Latency      419.30us   143.31us     7.43ms
    HTTP codes:
      1xx - 0, 2xx - 95823, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4177
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4177
    Throughput:    17.96MB/s
  ```


