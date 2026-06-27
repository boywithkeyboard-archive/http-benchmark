## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `71041` | `3863` | `83333` |
| **84%** | [Hyper Express](#hyper-express) | `59473` | `3395` | `67498` |
| **29%** | [Hono](#hono) | `20876` | `6526` | `30477` |
| **28%** | [Node (Default)](#node-default) | `19823` | `5016` | `65027` |
| **27%** | [Fastify](#fastify) | `19423` | `3946` | `33625` |
| **27%** | [Koa](#koa) | `19262` | `9126` | `70899` |
| **10%** | [Carbon](#carbon) | `7368` | `1234` | `10368` |
| **9%** | [Express](#express) | `6167` | `1075` | `8296` |


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
    Reqs/sec      8243.01    7197.58   77914.56
    Latency        6.06ms     4.47ms   382.76ms
    HTTP codes:
      1xx - 0, 2xx - 88197, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11803
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11803
    Throughput:     1.65MB/s
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
    Reqs/sec      6139.04    1068.74    8318.18
    Latency        8.14ms     3.81ms   363.19ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.76MB/s
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
    Reqs/sec     20426.02    5792.15   36062.55
    Latency        2.45ms     2.16ms   194.37ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.63MB/s
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
    Reqs/sec     20294.05    6374.80   30366.50
    Latency        2.46ms     2.14ms   189.05ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.58MB/s
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
    Reqs/sec     58368.01    3276.47   63918.44
    Latency        0.85ms    91.60us     3.38ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.29MB/s
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
    Reqs/sec     17678.08    7417.03   63609.39
    Latency        2.82ms     2.38ms   208.49ms
    HTTP codes:
      1xx - 0, 2xx - 93363, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6637
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6637
    Throughput:     3.73MB/s
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
    Reqs/sec     19991.32    5006.29   61081.77
    Latency        2.50ms     2.06ms   174.22ms
    HTTP codes:
      1xx - 0, 2xx - 97158, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2842
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2842
    Throughput:     4.45MB/s
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
    Reqs/sec     70465.19    4008.54   80237.18
    Latency      706.90us   149.57us     7.15ms
    HTTP codes:
      1xx - 0, 2xx - 97573, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2427
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2427
    Throughput:    10.88MB/s
  ```


