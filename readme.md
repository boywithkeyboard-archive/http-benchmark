## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72959` | `6828` | `80084` |
| **85%** | [Hyper Express](#hyper-express) | `61744` | `4692` | `84024` |
| **34%** | [Node (Default)](#node-default) | `24464` | `7324` | `53076` |
| **33%** | [Fastify](#fastify) | `23726` | `7323` | `34392` |
| **26%** | [Hono](#hono) | `19235` | `5543` | `28553` |
| **23%** | [Koa](#koa) | `16876` | `5974` | `46101` |
| **11%** | [Carbon](#carbon) | `7993` | `1427` | `10211` |
| **8%** | [Express](#express) | `6124` | `983` | `8123` |


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
    Reqs/sec      8286.54    3985.62   47775.57
    Latency        6.02ms     4.44ms   381.50ms
    HTTP codes:
      1xx - 0, 2xx - 93989, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6011
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6011
    Throughput:     1.77MB/s
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
    Reqs/sec      6072.37     961.70    8120.60
    Latency        8.23ms     3.81ms   360.90ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.74MB/s
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
    Reqs/sec     24342.64    7794.24   34893.06
    Latency        2.05ms     2.12ms   192.03ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.53MB/s
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
    Reqs/sec     19775.14    5510.81   28811.78
    Latency        2.52ms     2.06ms   187.01ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.47MB/s
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
    Reqs/sec     61429.53    3808.62   69454.69
    Latency      811.48us    77.41us     3.73ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.72MB/s
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
    Reqs/sec     18398.38    6959.83   57744.92
    Latency        2.71ms     2.48ms   218.31ms
    HTTP codes:
      1xx - 0, 2xx - 93329, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6671
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6671
    Throughput:     3.88MB/s
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
    Reqs/sec     24543.62    7026.34   54516.28
    Latency        2.03ms     2.02ms   175.01ms
    HTTP codes:
      1xx - 0, 2xx - 97524, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2476
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2457
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 19
    Throughput:     5.48MB/s
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
    Reqs/sec     71739.10    6048.89   83131.56
    Latency      692.56us   246.91us    14.64ms
    HTTP codes:
      1xx - 0, 2xx - 96200, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3800
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3800
    Throughput:    10.92MB/s
  ```


