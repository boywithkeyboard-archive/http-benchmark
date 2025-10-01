## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75365` | `3307` | `84774` |
| **83%** | [Hyper Express](#hyper-express) | `62767` | `3837` | `65035` |
| **34%** | [Node (Default)](#node-default) | `25929` | `8479` | `63833` |
| **32%** | [Fastify](#fastify) | `23947` | `7330` | `35707` |
| **27%** | [Hono](#hono) | `20544` | `5672` | `29695` |
| **24%** | [Koa](#koa) | `17727` | `7810` | `72180` |
| **11%** | [Carbon](#carbon) | `8225` | `1486` | `10486` |
| **8%** | [Express](#express) | `6255` | `1029` | `8315` |


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
    Reqs/sec      8653.78    6426.66   83083.15
    Latency        5.76ms     4.33ms   371.39ms
    HTTP codes:
      1xx - 0, 2xx - 90820, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9180
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9180
    Throughput:     1.79MB/s
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
    Reqs/sec      6326.48    1059.69    8339.68
    Latency        7.90ms     3.71ms   355.74ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.81MB/s
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
    Reqs/sec     23127.19    6827.28   35576.37
    Latency        2.16ms     2.03ms   182.83ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.24MB/s
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
    Reqs/sec     21057.65    5732.42   29618.80
    Latency        2.37ms     1.88ms   172.03ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.76MB/s
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
    Reqs/sec     61868.21    3075.36   66672.51
    Latency      806.07us    68.35us     2.76ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.79MB/s
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
    Reqs/sec     19068.13    8827.06   78293.44
    Latency        2.62ms     2.48ms   214.48ms
    HTTP codes:
      1xx - 0, 2xx - 91346, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8654
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8654
    Throughput:     3.93MB/s
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
    Reqs/sec     25752.86    8134.31   65130.44
    Latency        1.94ms     1.98ms   169.66ms
    HTTP codes:
      1xx - 0, 2xx - 97316, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2684
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2684
    Throughput:     5.73MB/s
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
    Reqs/sec     75185.19    3771.00   84284.14
    Latency      662.93us   127.44us     6.06ms
    HTTP codes:
      1xx - 0, 2xx - 97447, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2553
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2553
    Throughput:    11.59MB/s
  ```


