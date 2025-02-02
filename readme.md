## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73278` | `5834` | `83115` |
| **84%** | [Hyper Express](#hyper-express) | `61725` | `3866` | `66263` |
| **35%** | [Node (Default)](#node-default) | `25634` | `7327` | `47927` |
| **32%** | [Fastify](#fastify) | `23697` | `7030` | `35591` |
| **29%** | [Hono](#hono) | `21186` | `5254` | `29169` |
| **25%** | [Koa](#koa) | `18625` | `6551` | `52134` |
| **11%** | [Carbon](#carbon) | `8204` | `1379` | `10252` |
| **9%** | [Express](#express) | `6254` | `1015` | `8290` |


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
    Reqs/sec      8708.61    4402.13   54050.97
    Latency        5.73ms     4.39ms   380.34ms
    HTTP codes:
      1xx - 0, 2xx - 93145, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6855
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6855
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
    Reqs/sec      6343.45    1016.89    8470.07
    Latency        7.88ms     3.67ms   349.09ms
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
    Reqs/sec     24904.62    7776.10   35671.80
    Latency        2.00ms     2.02ms   180.03ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.65MB/s
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
    Reqs/sec     20432.54    5761.86   29660.25
    Latency        2.44ms     2.12ms   189.06ms
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
    Reqs/sec     62105.54    3517.19   65097.78
    Latency      802.91us    71.84us     3.22ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.82MB/s
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
    Reqs/sec     18302.81    6509.66   55618.91
    Latency        2.73ms     2.42ms   210.24ms
    HTTP codes:
      1xx - 0, 2xx - 94012, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5988
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5988
    Throughput:     3.89MB/s
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
    Reqs/sec     25884.28    7868.78   57028.19
    Latency        1.93ms     1.95ms   170.69ms
    HTTP codes:
      1xx - 0, 2xx - 97296, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2704
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2704
    Throughput:     5.76MB/s
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
    Reqs/sec     74167.91    5510.06   84338.74
    Latency      671.88us   154.88us     7.52ms
    HTTP codes:
      1xx - 0, 2xx - 98020, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1980
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1980
    Throughput:    11.50MB/s
  ```


