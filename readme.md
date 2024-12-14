## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75694` | `5723` | `84044` |
| **84%** | [Hyper Express](#hyper-express) | `63375` | `4104` | `85246` |
| **32%** | [Node (Default)](#node-default) | `24521` | `6617` | `46259` |
| **32%** | [Fastify](#fastify) | `24200` | `7231` | `36477` |
| **26%** | [Hono](#hono) | `19839` | `5750` | `29530` |
| **24%** | [Koa](#koa) | `18177` | `5801` | `49906` |
| **11%** | [Carbon](#carbon) | `8346` | `1407` | `10561` |
| **9%** | [Express](#express) | `6441` | `958` | `8413` |


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
    Reqs/sec      8515.33    3404.12   51818.21
    Latency        5.86ms     4.39ms   380.57ms
    HTTP codes:
      1xx - 0, 2xx - 95209, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4791
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4791
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
    Reqs/sec      6506.25    1051.89    8500.85
    Latency        7.68ms     3.65ms   346.87ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.86MB/s
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
    Reqs/sec     23937.81    7070.99   37021.10
    Latency        2.09ms     1.93ms   178.57ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.42MB/s
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
    Reqs/sec     20166.15    5430.46   28901.90
    Latency        2.48ms     1.94ms   176.03ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.56MB/s
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
    Reqs/sec     62730.15    3919.42   65419.57
    Latency      794.67us    69.46us     3.12ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.91MB/s
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
    Reqs/sec     18776.10    6797.97   58056.81
    Latency        2.66ms     2.36ms   201.88ms
    HTTP codes:
      1xx - 0, 2xx - 93566, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6434
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6434
    Throughput:     3.97MB/s
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
    Reqs/sec     26185.84    7996.84   59430.67
    Latency        1.91ms     1.83ms   158.60ms
    HTTP codes:
      1xx - 0, 2xx - 97379, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2621
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2621
    Throughput:     5.83MB/s
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
    Reqs/sec     75115.43    6542.86   87714.78
    Latency      663.41us   174.28us    10.83ms
    HTTP codes:
      1xx - 0, 2xx - 97267, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2733
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2733
    Throughput:    11.56MB/s
  ```


