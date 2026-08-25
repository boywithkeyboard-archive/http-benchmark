## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72164` | `4225` | `87006` |
| **85%** | [Hyper Express](#hyper-express) | `61190` | `3746` | `68798` |
| **32%** | [Node (Default)](#node-default) | `23275` | `6649` | `71521` |
| **31%** | [Fastify](#fastify) | `22067` | `6038` | `36889` |
| **31%** | [Hono](#hono) | `22062` | `6406` | `30660` |
| **27%** | [Koa](#koa) | `19596` | `9549` | `81197` |
| **11%** | [Carbon](#carbon) | `7586` | `1211` | `10535` |
| **9%** | [Express](#express) | `6342` | `1064` | `8414` |


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
    Reqs/sec      8267.83    5941.05   71610.38
    Latency        6.03ms     4.58ms   389.92ms
    HTTP codes:
      1xx - 0, 2xx - 91177, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8823
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8823
    Throughput:     1.71MB/s
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
    Reqs/sec      6481.75    1112.55    8755.33
    Latency        7.71ms     3.73ms   354.66ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.85MB/s
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
    Reqs/sec     21769.73    5793.51   36932.50
    Latency        2.30ms     2.03ms   183.92ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.93MB/s
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
    Reqs/sec     22769.80    6591.52   30897.55
    Latency        2.19ms     2.15ms   190.71ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.15MB/s
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
    Reqs/sec     61804.97    4549.58   82150.07
    Latency      809.61us    94.12us     3.06ms
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
    Reqs/sec     19875.20    8383.59   61976.55
    Latency        2.51ms     2.40ms   210.38ms
    HTTP codes:
      1xx - 0, 2xx - 92754, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7246
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7246
    Throughput:     4.17MB/s
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
    Reqs/sec     22371.37    6079.48   68159.89
    Latency        2.23ms     1.93ms   160.80ms
    HTTP codes:
      1xx - 0, 2xx - 96509, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3491
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3491
    Throughput:     4.95MB/s
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
    Reqs/sec     72782.61    3488.92   81896.71
    Latency      683.76us   192.77us    14.79ms
    HTTP codes:
      1xx - 0, 2xx - 95673, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4327
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4327
    Throughput:    11.02MB/s
  ```


