## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74304` | `5693` | `84992` |
| **83%** | [Hyper Express](#hyper-express) | `61960` | `3860` | `65840` |
| **35%** | [Node (Default)](#node-default) | `25947` | `8470` | `52731` |
| **31%** | [Fastify](#fastify) | `23191` | `7089` | `35096` |
| **28%** | [Hono](#hono) | `20460` | `6052` | `29244` |
| **24%** | [Koa](#koa) | `18074` | `6577` | `52452` |
| **11%** | [Carbon](#carbon) | `8070` | `1355` | `10325` |
| **8%** | [Express](#express) | `6231` | `967` | `8199` |


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
    Reqs/sec      8442.26    3225.85   48086.61
    Latency        5.91ms     4.29ms   370.69ms
    HTTP codes:
      1xx - 0, 2xx - 95845, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4155
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4155
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
    Reqs/sec      6257.21    1001.52    8392.11
    Latency        7.99ms     3.78ms   365.49ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     24464.33    7629.13   34685.51
    Latency        2.04ms     2.12ms   189.04ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.55MB/s
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
    Reqs/sec     21042.87    6049.02   28667.63
    Latency        2.37ms     2.05ms   186.39ms
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
    Reqs/sec     62302.76    3209.10   74605.63
    Latency      799.43us    82.03us     3.16ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.86MB/s
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
    Reqs/sec     17712.97    6657.90   62575.04
    Latency        2.82ms     2.39ms   210.26ms
    HTTP codes:
      1xx - 0, 2xx - 93662, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6338
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6338
    Throughput:     3.75MB/s
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
    Reqs/sec     26945.44    7960.64   46712.39
    Latency        1.85ms     1.84ms   155.64ms
    HTTP codes:
      1xx - 0, 2xx - 97378, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2622
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2622
    Throughput:     6.00MB/s
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
    Reqs/sec     76253.22    7951.02  113640.67
    Latency      658.20us   191.81us    11.07ms
    HTTP codes:
      1xx - 0, 2xx - 97571, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2429
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2429
    Throughput:    11.69MB/s
  ```


