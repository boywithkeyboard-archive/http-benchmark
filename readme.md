## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `102372` | `3722` | `116109` |
| **83%** | [Hyper Express](#hyper-express) | `85091` | `6616` | `93451` |
| **45%** | [Fastify](#fastify) | `46304` | `14848` | `68132` |
| **45%** | [Node (Default)](#node-default) | `46096` | `15079` | `95775` |
| **38%** | [Hono](#hono) | `39250` | `11568` | `59821` |
| **35%** | [Koa](#koa) | `35772` | `14596` | `96250` |
| **12%** | [Carbon](#carbon) | `11777` | `2760` | `17007` |
| **9%** | [Express](#express) | `9270` | `1960` | `13414` |


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
    Reqs/sec     12586.27    9340.73  109372.51
    Latency        3.97ms     3.38ms   290.50ms
    HTTP codes:
      1xx - 0, 2xx - 90811, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9189
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9189
    Throughput:     2.59MB/s
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
    Reqs/sec     10345.55    8789.41   91819.42
    Latency        4.83ms     2.97ms   272.99ms
    HTTP codes:
      1xx - 0, 2xx - 89548, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10452
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10452
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
    Reqs/sec     49207.79   16939.81   67180.27
    Latency        1.02ms     1.43ms   129.24ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    11.15MB/s
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
    Reqs/sec     38048.85   11433.65   60045.82
    Latency        1.31ms     1.64ms   142.06ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.60MB/s
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
    Reqs/sec     87509.98    3780.25   90995.44
    Latency      569.16us    48.78us     2.05ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    12.44MB/s
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
    Reqs/sec     39159.13   16367.27   93553.55
    Latency        1.27ms     1.70ms   142.29ms
    HTTP codes:
      1xx - 0, 2xx - 92472, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7528
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7528
    Throughput:     8.21MB/s
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
    Reqs/sec     47410.77   14099.09   87473.52
    Latency        1.05ms     1.46ms   114.59ms
    HTTP codes:
      1xx - 0, 2xx - 96505, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3495
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3495
    Throughput:    10.48MB/s
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
    Reqs/sec    101105.91    3663.78  108385.36
    Latency      492.40us   187.78us    10.28ms
    HTTP codes:
      1xx - 0, 2xx - 95665, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4335
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4335
    Throughput:    15.31MB/s
  ```


