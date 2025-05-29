## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73783` | `5725` | `97267` |
| **83%** | [Hyper Express](#hyper-express) | `61361` | `3478` | `63651` |
| **37%** | [Node (Default)](#node-default) | `27243` | `8957` | `60528` |
| **33%** | [Fastify](#fastify) | `24660` | `8140` | `38797` |
| **29%** | [Hono](#hono) | `21104` | `6044` | `30943` |
| **26%** | [Koa](#koa) | `19124` | `6653` | `53633` |
| **11%** | [Carbon](#carbon) | `8212` | `1413` | `10319` |
| **9%** | [Express](#express) | `6419` | `1030` | `8433` |


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
    Reqs/sec      8476.70    3971.59   50538.49
    Latency        5.89ms     4.44ms   379.92ms
    HTTP codes:
      1xx - 0, 2xx - 94499, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5501
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5501
    Throughput:     1.82MB/s
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
    Reqs/sec      6398.33    1053.11    8438.90
    Latency        7.81ms     3.72ms   357.05ms
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
    Reqs/sec     23936.68    7259.14   35835.78
    Latency        2.09ms     2.10ms   188.16ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.43MB/s
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
    Reqs/sec     21029.83    5727.05   30472.52
    Latency        2.38ms     2.12ms   189.50ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.75MB/s
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
    Reqs/sec     61864.23    3256.36   65762.00
    Latency      805.97us    70.16us     2.76ms
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
    Reqs/sec     18980.52    7040.80   54504.78
    Latency        2.63ms     2.41ms   209.79ms
    HTTP codes:
      1xx - 0, 2xx - 93317, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6683
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6683
    Throughput:     4.01MB/s
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
    Reqs/sec     26256.54    8299.35   54238.29
    Latency        1.90ms     1.95ms   170.43ms
    HTTP codes:
      1xx - 0, 2xx - 97671, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2329
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2329
    Throughput:     5.88MB/s
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
    Reqs/sec     73746.24    4892.13   84501.17
    Latency      675.66us   151.55us     6.83ms
    HTTP codes:
      1xx - 0, 2xx - 97333, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2667
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2667
    Throughput:    11.35MB/s
  ```


