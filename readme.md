## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75013` | `3262` | `83658` |
| **84%** | [Hyper Express](#hyper-express) | `62912` | `3360` | `64967` |
| **34%** | [Node (Default)](#node-default) | `25822` | `8347` | `69425` |
| **33%** | [Fastify](#fastify) | `25033` | `8022` | `36522` |
| **27%** | [Hono](#hono) | `19909` | `5165` | `29239` |
| **26%** | [Koa](#koa) | `19325` | `8192` | `67107` |
| **11%** | [Carbon](#carbon) | `8368` | `1452` | `10390` |
| **9%** | [Express](#express) | `6444` | `1063` | `8479` |


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
    Reqs/sec      8774.42    4947.58   58581.37
    Latency        5.68ms     4.44ms   383.25ms
    HTTP codes:
      1xx - 0, 2xx - 92315, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7685
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7685
    Throughput:     1.84MB/s
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
    Reqs/sec      6927.62    5861.99   78601.09
    Latency        7.20ms     3.89ms   358.34ms
    HTTP codes:
      1xx - 0, 2xx - 90274, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9726
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9726
    Throughput:     1.79MB/s
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
    Reqs/sec     25394.29    8236.61   36742.50
    Latency        1.97ms     1.97ms   174.90ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.75MB/s
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
    Reqs/sec     21278.75    5729.84   29592.21
    Latency        2.35ms     1.95ms   174.35ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.81MB/s
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
    Reqs/sec     63001.13    4018.44   67424.58
    Latency      791.00us    72.15us     2.77ms
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
    Reqs/sec     18855.38    9932.73   79893.83
    Latency        2.65ms     2.35ms   207.81ms
    HTTP codes:
      1xx - 0, 2xx - 89434, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10566
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10566
    Throughput:     3.81MB/s
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
    Reqs/sec     25319.38    7459.77   58395.16
    Latency        1.97ms     1.94ms   169.62ms
    HTTP codes:
      1xx - 0, 2xx - 97600, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2400
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2400
    Throughput:     5.66MB/s
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
    Reqs/sec     74864.03    3390.11   78456.68
    Latency      665.47us   150.23us     6.12ms
    HTTP codes:
      1xx - 0, 2xx - 97100, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2900
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2900
    Throughput:    11.50MB/s
  ```


