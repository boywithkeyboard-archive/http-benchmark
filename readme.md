## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72718` | `4802` | `86195` |
| **81%** | [Hyper Express](#hyper-express) | `59021` | `3900` | `75536` |
| **28%** | [Node (Default)](#node-default) | `20373` | `5671` | `70145` |
| **28%** | [Fastify](#fastify) | `20177` | `5624` | `37043` |
| **27%** | [Hono](#hono) | `19930` | `5633` | `29814` |
| **25%** | [Koa](#koa) | `18427` | `9491` | `77460` |
| **10%** | [Carbon](#carbon) | `7392` | `1219` | `10238` |
| **9%** | [Express](#express) | `6220` | `1172` | `8124` |


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
    Reqs/sec      7985.85    6124.22   71143.86
    Latency        6.25ms     4.51ms   389.63ms
    HTTP codes:
      1xx - 0, 2xx - 90294, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9706
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9706
    Throughput:     1.64MB/s
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
    Reqs/sec      6382.44    1193.25    8316.49
    Latency        7.83ms     3.87ms   371.83ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.83MB/s
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
    Reqs/sec     19958.02    5618.00   37598.37
    Latency        2.50ms     2.04ms   184.78ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.52MB/s
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
    Reqs/sec     21417.93    6660.05   31491.15
    Latency        2.33ms     2.07ms   183.50ms
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
    Reqs/sec     59545.27    3714.62   66332.17
    Latency      837.45us    86.42us     3.77ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.46MB/s
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
    Reqs/sec     17377.33    8327.98   76091.90
    Latency        2.87ms     2.53ms   222.92ms
    HTTP codes:
      1xx - 0, 2xx - 92314, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7686
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7686
    Throughput:     3.63MB/s
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
    Reqs/sec     20946.20    6648.80   68084.80
    Latency        2.38ms     2.03ms   176.05ms
    HTTP codes:
      1xx - 0, 2xx - 96475, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3525
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3525
    Throughput:     4.63MB/s
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
    Reqs/sec     69696.39    5755.25   95715.02
    Latency      714.62us   209.88us    12.16ms
    HTTP codes:
      1xx - 0, 2xx - 96817, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3183
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3183
    Throughput:    10.68MB/s
  ```


