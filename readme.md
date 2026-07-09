## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `69383` | `4721` | `91414` |
| **82%** | [Hyper Express](#hyper-express) | `57195` | `3158` | `65794` |
| **30%** | [Hono](#hono) | `21083` | `6259` | `30650` |
| **29%** | [Fastify](#fastify) | `19778` | `4534` | `37167` |
| **28%** | [Node (Default)](#node-default) | `19704` | `5414` | `68685` |
| **28%** | [Koa](#koa) | `19388` | `9384` | `73355` |
| **10%** | [Carbon](#carbon) | `7181` | `1228` | `10194` |
| **9%** | [Express](#express) | `6176` | `1120` | `10405` |


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
    Reqs/sec      7719.69    5176.37   65276.66
    Latency        6.47ms     4.81ms   404.43ms
    HTTP codes:
      1xx - 0, 2xx - 92287, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7713
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7713
    Throughput:     1.62MB/s
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
    Reqs/sec      6103.73    1117.80    8193.75
    Latency        8.19ms     4.06ms   387.02ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.75MB/s
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
    Reqs/sec     20665.39    5782.74   35807.61
    Latency        2.42ms     2.08ms   187.47ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.69MB/s
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
    Reqs/sec     21160.38    6851.67   30156.55
    Latency        2.36ms     2.25ms   200.29ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.78MB/s
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
    Reqs/sec     57984.05    3410.70   64685.18
    Latency        0.86ms   107.37us     4.82ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.24MB/s
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
    Reqs/sec     19123.90    9614.41   80578.76
    Latency        2.61ms     2.49ms   214.38ms
    HTTP codes:
      1xx - 0, 2xx - 90359, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9641
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9641
    Throughput:     3.91MB/s
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
    Reqs/sec     19991.90    5141.54   72180.43
    Latency        2.50ms     2.13ms   182.67ms
    HTTP codes:
      1xx - 0, 2xx - 96698, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3302
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3302
    Throughput:     4.43MB/s
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
    Reqs/sec     67882.49    4156.97   84028.68
    Latency      733.62us   176.18us     9.95ms
    HTTP codes:
      1xx - 0, 2xx - 96748, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3252
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3252
    Throughput:    10.40MB/s
  ```


