## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `71578` | `5882` | `89509` |
| **83%** | [Hyper Express](#hyper-express) | `59616` | `3847` | `66371` |
| **30%** | [Node (Default)](#node-default) | `21344` | `7063` | `62534` |
| **29%** | [Fastify](#fastify) | `21030` | `5961` | `36225` |
| **29%** | [Hono](#hono) | `20437` | `6360` | `29043` |
| **25%** | [Koa](#koa) | `18120` | `7941` | `61114` |
| **10%** | [Carbon](#carbon) | `7261` | `1219` | `10023` |
| **9%** | [Express](#express) | `6120` | `1113` | `8238` |


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
    Reqs/sec      7734.46    4992.58   74055.72
    Latency        6.46ms     4.69ms   404.84ms
    HTTP codes:
      1xx - 0, 2xx - 92940, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7060
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7060
    Throughput:     1.63MB/s
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
    Reqs/sec      6065.08    1095.33    8245.51
    Latency        8.24ms     3.96ms   378.43ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.73MB/s
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
    Reqs/sec     21337.03    6485.92   36749.79
    Latency        2.34ms     2.16ms   192.95ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.85MB/s
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
    Reqs/sec     20483.15    6064.37   29896.53
    Latency        2.44ms     2.17ms   192.45ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.62MB/s
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
    Reqs/sec     60310.43    3896.95   69313.11
    Latency      828.08us    89.62us     4.09ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.55MB/s
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
    Reqs/sec     17415.83    8253.74   66242.38
    Latency        2.86ms     2.53ms   224.15ms
    HTTP codes:
      1xx - 0, 2xx - 92390, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7610
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7610
    Throughput:     3.64MB/s
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
    Reqs/sec     20603.03    5128.03   57254.92
    Latency        2.42ms     2.08ms   175.45ms
    HTTP codes:
      1xx - 0, 2xx - 97452, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2548
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2548
    Throughput:     4.61MB/s
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
    Reqs/sec     73900.62    5861.83   88578.36
    Latency      674.09us   168.96us     6.53ms
    HTTP codes:
      1xx - 0, 2xx - 97335, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2665
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2665
    Throughput:    11.39MB/s
  ```


