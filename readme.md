## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74439` | `2987` | `92309` |
| **83%** | [Hyper Express](#hyper-express) | `62057` | `4177` | `76075` |
| **34%** | [Node (Default)](#node-default) | `25019` | `7434` | `67237` |
| **31%** | [Fastify](#fastify) | `23123` | `6313` | `36437` |
| **27%** | [Hono](#hono) | `20041` | `5263` | `31266` |
| **25%** | [Koa](#koa) | `18361` | `8504` | `80333` |
| **11%** | [Carbon](#carbon) | `8411` | `1447` | `10584` |
| **9%** | [Express](#express) | `6499` | `1100` | `8450` |


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
    Reqs/sec      9052.38    6682.04   81595.96
    Latency        5.51ms     4.26ms   366.78ms
    HTTP codes:
      1xx - 0, 2xx - 90199, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9801
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9801
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
    Reqs/sec      6461.28    1061.82    8324.81
    Latency        7.73ms     3.59ms   348.56ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     22819.68    6191.09   35354.38
    Latency        2.19ms     2.04ms   182.53ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.18MB/s
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
    Reqs/sec     19282.93    4688.46   30627.49
    Latency        2.59ms     2.02ms   182.53ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.36MB/s
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
    Reqs/sec     60957.37    3839.75   64078.30
    Latency      818.25us    77.66us     3.38ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.66MB/s
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
    Reqs/sec     18191.93    8682.11   82725.32
    Latency        2.74ms     2.32ms   210.00ms
    HTTP codes:
      1xx - 0, 2xx - 91228, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8772
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8772
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
    Reqs/sec     26189.40    8252.64   67505.65
    Latency        1.91ms     1.90ms   164.58ms
    HTTP codes:
      1xx - 0, 2xx - 97092, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2908
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2908
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
    Reqs/sec     74656.62    2395.65   84650.03
    Latency      666.89us   123.21us     4.88ms
    HTTP codes:
      1xx - 0, 2xx - 97246, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2754
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2754
    Throughput:    11.49MB/s
  ```


