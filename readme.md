## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `117540` | `10612` | `132179` |
| **85%** | [Hyper Express](#hyper-express) | `99673` | `6960` | `103322` |
| **33%** | [Node (Default)](#node-default) | `39029` | `11789` | `112433` |
| **32%** | [Fastify](#fastify) | `37060` | `8850` | `55178` |
| **28%** | [Hono](#hono) | `32717` | `6699` | `47014` |
| **27%** | [Koa](#koa) | `32036` | `14270` | `108371` |
| **11%** | [Carbon](#carbon) | `12559` | `2515` | `16557` |
| **8%** | [Express](#express) | `8904` | `1626` | `11698` |


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
    Reqs/sec     13602.70    9525.17   93544.49
    Latency        3.66ms     3.83ms   323.58ms
    HTTP codes:
      1xx - 0, 2xx - 89036, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10964
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10964
    Throughput:     2.75MB/s
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
    Reqs/sec     10123.06   10682.09  117283.87
    Latency        4.93ms     3.34ms   302.89ms
    HTTP codes:
      1xx - 0, 2xx - 86066, 3xx - 0, 4xx - 0, 5xx - 0
      others - 13934
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 13934
    Throughput:     2.50MB/s
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
    Reqs/sec     36436.59    8583.22   54694.22
    Latency        1.37ms     1.49ms   129.14ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.27MB/s
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
    Reqs/sec     32335.16    7227.83   47575.34
    Latency        1.55ms     1.48ms   127.74ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.29MB/s
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
    Reqs/sec     99210.36    7015.81  108049.58
    Latency      502.23us    55.69us     2.83ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    14.09MB/s
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
    Reqs/sec     31702.44   12683.08  101720.79
    Latency        1.57ms     1.73ms   148.25ms
    HTTP codes:
      1xx - 0, 2xx - 91013, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8987
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8987
    Throughput:     6.53MB/s
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
    Reqs/sec     39820.10   11269.80  102012.14
    Latency        1.25ms     1.37ms   118.17ms
    HTTP codes:
      1xx - 0, 2xx - 94750, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5250
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5250
    Throughput:     8.65MB/s
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
    Reqs/sec    119898.56    8381.47  134183.67
    Latency      413.89us   184.19us    11.61ms
    HTTP codes:
      1xx - 0, 2xx - 96125, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3875
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3875
    Throughput:    18.24MB/s
  ```


