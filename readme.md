## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `83251` | `2701` | `90764` |
| **85%** | [Hyper Express](#hyper-express) | `70906` | `2722` | `76075` |
| **43%** | [Fastify](#fastify) | `36016` | `11259` | `53930` |
| **42%** | [Node (Default)](#node-default) | `34578` | `9366` | `67962` |
| **36%** | [Hono](#hono) | `29777` | `8546` | `48479` |
| **36%** | [Koa](#koa) | `29629` | `12446` | `83889` |
| **11%** | [Carbon](#carbon) | `9342` | `2392` | `13508` |
| **9%** | [Express](#express) | `7637` | `1788` | `10768` |


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
    Reqs/sec     10348.61    8189.94   84855.26
    Latency        4.82ms     4.12ms   355.70ms
    HTTP codes:
      1xx - 0, 2xx - 89019, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10981
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10981
    Throughput:     2.09MB/s
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
    Reqs/sec      8402.54    7476.61   79310.22
    Latency        5.94ms     3.78ms   346.88ms
    HTTP codes:
      1xx - 0, 2xx - 88707, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11293
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11293
    Throughput:     2.14MB/s
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
    Reqs/sec     34119.37    9710.08   52982.75
    Latency        1.46ms     1.93ms   168.62ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.74MB/s
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
    Reqs/sec     30999.66    9402.82   48808.18
    Latency        1.61ms     1.89ms   168.73ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     6.99MB/s
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
    Reqs/sec     70645.84    3204.26   74715.40
    Latency      706.04us    56.45us     2.26ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    10.04MB/s
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
    Reqs/sec     30360.74   14769.37   89622.86
    Latency        1.64ms     2.26ms   197.03ms
    HTTP codes:
      1xx - 0, 2xx - 88880, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11120
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11120
    Throughput:     6.11MB/s
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
    Reqs/sec     36739.70   11539.46   79863.63
    Latency        1.36ms     1.79ms   148.12ms
    HTTP codes:
      1xx - 0, 2xx - 96355, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3645
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3645
    Throughput:     8.11MB/s
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
    Reqs/sec     81632.05    2646.59   84915.51
    Latency      609.77us   173.45us     9.49ms
    HTTP codes:
      1xx - 0, 2xx - 96441, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3559
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3559
    Throughput:    12.47MB/s
  ```


