## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `69512` | `3985` | `80690` |
| **82%** | [Hyper Express](#hyper-express) | `56872` | `4339` | `64059` |
| **31%** | [Node (Default)](#node-default) | `21423` | `5091` | `59829` |
| **29%** | [Fastify](#fastify) | `20361` | `4687` | `36587` |
| **29%** | [Hono](#hono) | `20266` | `6366` | `30757` |
| **25%** | [Koa](#koa) | `17286` | `8520` | `72818` |
| **11%** | [Carbon](#carbon) | `7838` | `1468` | `10375` |
| **9%** | [Express](#express) | `6351` | `1176` | `8365` |


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
    Reqs/sec      8460.19    5958.24   73309.93
    Latency        5.89ms     4.38ms   377.27ms
    HTTP codes:
      1xx - 0, 2xx - 91346, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8654
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8654
    Throughput:     1.76MB/s
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
    Reqs/sec      6295.34    1199.42    8302.80
    Latency        7.94ms     3.77ms   364.22ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.80MB/s
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
    Reqs/sec     20694.86    5175.01   37045.07
    Latency        2.41ms     2.20ms   198.90ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.70MB/s
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
    Reqs/sec     21706.49    7151.07   30803.23
    Latency        2.30ms     2.09ms   186.21ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.91MB/s
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
    Reqs/sec     57491.42    3257.93   67913.89
    Latency        0.87ms    85.13us     4.03ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.17MB/s
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
    Reqs/sec     17412.38    7712.55   72184.10
    Latency        2.87ms     2.46ms   219.66ms
    HTTP codes:
      1xx - 0, 2xx - 93162, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6838
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6838
    Throughput:     3.67MB/s
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
    Reqs/sec     21822.19    5450.86   68631.02
    Latency        2.29ms     1.96ms   167.80ms
    HTTP codes:
      1xx - 0, 2xx - 96730, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3270
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3270
    Throughput:     4.83MB/s
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
    Reqs/sec     69050.65    3719.46   81584.93
    Latency      720.79us   213.70us    12.41ms
    HTTP codes:
      1xx - 0, 2xx - 95807, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4193
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4193
    Throughput:    10.47MB/s
  ```


