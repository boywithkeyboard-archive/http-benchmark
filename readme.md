## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74397` | `5045` | `81128` |
| **86%** | [Hyper Express](#hyper-express) | `63930` | `4021` | `72080` |
| **36%** | [Node (Default)](#node-default) | `26920` | `7035` | `45620` |
| **32%** | [Fastify](#fastify) | `24118` | `7160` | `36753` |
| **29%** | [Hono](#hono) | `21362` | `5758` | `30508` |
| **25%** | [Koa](#koa) | `18398` | `6603` | `55580` |
| **11%** | [Carbon](#carbon) | `8317` | `1427` | `10385` |
| **9%** | [Express](#express) | `6401` | `1034` | `8340` |


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
    Reqs/sec      8848.40    4942.45   55540.53
    Latency        5.64ms     4.37ms   371.67ms
    HTTP codes:
      1xx - 0, 2xx - 91934, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8066
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8066
    Throughput:     1.85MB/s
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
    Reqs/sec      7034.00    4658.73   62235.08
    Latency        7.10ms     3.89ms   354.13ms
    HTTP codes:
      1xx - 0, 2xx - 91820, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8180
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8180
    Throughput:     1.85MB/s
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
    Reqs/sec     24589.86    7509.90   36749.46
    Latency        2.03ms     1.95ms   175.83ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.59MB/s
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
    Reqs/sec     20885.47    5474.78   29600.50
    Latency        2.39ms     2.01ms   179.18ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.72MB/s
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
    Reqs/sec     62789.30    4044.04   69814.94
    Latency      794.19us    71.61us     2.95ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.92MB/s
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
    Reqs/sec     18872.49    7060.43   53330.59
    Latency        2.64ms     2.42ms   209.89ms
    HTTP codes:
      1xx - 0, 2xx - 93292, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6708
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6708
    Throughput:     3.98MB/s
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
    Reqs/sec     26808.18    7889.96   50814.57
    Latency        1.86ms     1.94ms   165.01ms
    HTTP codes:
      1xx - 0, 2xx - 97886, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2114
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2114
    Throughput:     6.01MB/s
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
    Reqs/sec     74551.62    5545.97   88265.41
    Latency      668.96us   143.53us     5.66ms
    HTTP codes:
      1xx - 0, 2xx - 97669, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2331
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2331
    Throughput:    11.51MB/s
  ```


