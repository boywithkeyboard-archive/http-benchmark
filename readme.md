## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74182` | `5568` | `83122` |
| **84%** | [Hyper Express](#hyper-express) | `62003` | `3301` | `64336` |
| **34%** | [Node (Default)](#node-default) | `25195` | `6530` | `46058` |
| **32%** | [Fastify](#fastify) | `23998` | `7073` | `36535` |
| **29%** | [Hono](#hono) | `21150` | `6036` | `30823` |
| **26%** | [Koa](#koa) | `19187` | `6803` | `55754` |
| **11%** | [Carbon](#carbon) | `8206` | `1359` | `10327` |
| **9%** | [Express](#express) | `6629` | `1097` | `8567` |


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
    Reqs/sec      8901.88    4392.23   54797.92
    Latency        5.61ms     4.22ms   365.61ms
    HTTP codes:
      1xx - 0, 2xx - 93694, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6306
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6306
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
    Reqs/sec      6516.12    1038.50    8440.17
    Latency        7.67ms     3.68ms   350.56ms
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
    Reqs/sec     24610.51    7430.86   37023.28
    Latency        2.03ms     2.05ms   183.22ms
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
    Reqs/sec     21958.89    6115.17   30173.22
    Latency        2.27ms     2.00ms   179.77ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.96MB/s
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
    Reqs/sec     62642.69    3773.37   67192.45
    Latency      796.11us    67.19us     3.41ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.90MB/s
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
    Reqs/sec     18515.14    6549.76   51363.53
    Latency        2.69ms     2.38ms   207.05ms
    HTTP codes:
      1xx - 0, 2xx - 94335, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5665
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5665
    Throughput:     3.95MB/s
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
    Reqs/sec     27676.28    8906.38   59521.73
    Latency        1.80ms     1.95ms   164.96ms
    HTTP codes:
      1xx - 0, 2xx - 96085, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3915
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3915
    Throughput:     6.10MB/s
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
    Reqs/sec     74334.48    5479.87   83396.07
    Latency      669.31us   194.11us     9.31ms
    HTTP codes:
      1xx - 0, 2xx - 96796, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3204
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3204
    Throughput:    11.39MB/s
  ```


