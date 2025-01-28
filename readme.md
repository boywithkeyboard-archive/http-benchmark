## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `76322` | `6014` | `87618` |
| **84%** | [Hyper Express](#hyper-express) | `64322` | `4036` | `74757` |
| **36%** | [Node (Default)](#node-default) | `27344` | `7714` | `47858` |
| **34%** | [Fastify](#fastify) | `25675` | `7833` | `37334` |
| **28%** | [Hono](#hono) | `21227` | `6131` | `30586` |
| **25%** | [Koa](#koa) | `19142` | `7026` | `56778` |
| **11%** | [Carbon](#carbon) | `8421` | `1447` | `10564` |
| **9%** | [Express](#express) | `6673` | `1091` | `8481` |


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
    Reqs/sec      8816.61    4318.40   54831.64
    Latency        5.66ms     4.21ms   364.16ms
    HTTP codes:
      1xx - 0, 2xx - 94208, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5792
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5792
    Throughput:     1.89MB/s
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
    Reqs/sec      7035.55    3772.31   49230.00
    Latency        7.10ms     3.81ms   348.77ms
    HTTP codes:
      1xx - 0, 2xx - 93899, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6101
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6101
    Throughput:     1.89MB/s
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
    Reqs/sec     24436.75    7320.18   37245.51
    Latency        2.04ms     2.00ms   177.14ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.54MB/s
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
    Reqs/sec     21318.46    5724.01   30427.04
    Latency        2.34ms     1.99ms   178.83ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.82MB/s
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
    Reqs/sec     64962.19    3979.69   72813.61
    Latency      767.44us    93.53us     7.33ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.23MB/s
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
    Reqs/sec     19344.77    7109.33   53163.10
    Latency        2.58ms     2.25ms   197.07ms
    HTTP codes:
      1xx - 0, 2xx - 93432, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6568
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6568
    Throughput:     4.09MB/s
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
    Reqs/sec     26182.73    7434.27   58341.45
    Latency        1.91ms     1.90ms   164.24ms
    HTTP codes:
      1xx - 0, 2xx - 97334, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2666
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2666
    Throughput:     5.82MB/s
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
    Reqs/sec     76328.23    5657.72   87914.62
    Latency      653.43us   142.91us     7.09ms
    HTTP codes:
      1xx - 0, 2xx - 97317, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2683
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2683
    Throughput:    11.74MB/s
  ```


