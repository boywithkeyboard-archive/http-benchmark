## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `71693` | `3638` | `80825` |
| **84%** | [Hyper Express](#hyper-express) | `59933` | `3797` | `66923` |
| **33%** | [Node (Default)](#node-default) | `23531` | `5887` | `52521` |
| **31%** | [Fastify](#fastify) | `22365` | `6389` | `36991` |
| **29%** | [Hono](#hono) | `20934` | `6326` | `29951` |
| **26%** | [Koa](#koa) | `18840` | `8854` | `71966` |
| **11%** | [Carbon](#carbon) | `8007` | `1412` | `10263` |
| **9%** | [Express](#express) | `6255` | `1052` | `8357` |


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
    Reqs/sec      8972.20    6298.43   73796.61
    Latency        5.56ms     4.42ms   379.94ms
    HTTP codes:
      1xx - 0, 2xx - 90367, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9633
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9633
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
    Reqs/sec      6335.75    1074.94    8343.37
    Latency        7.89ms     3.81ms   362.80ms
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
    Reqs/sec     22858.62    6723.81   36583.18
    Latency        2.18ms     2.18ms   195.89ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.19MB/s
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
    Reqs/sec     20895.94    6229.34   29425.35
    Latency        2.39ms     2.01ms   181.70ms
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
    Reqs/sec     59526.13    3626.08   68695.87
    Latency      838.88us    83.85us     2.94ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.45MB/s
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
    Reqs/sec     19249.89    8486.45   68132.06
    Latency        2.59ms     2.55ms   225.98ms
    HTTP codes:
      1xx - 0, 2xx - 92640, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7360
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7360
    Throughput:     4.03MB/s
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
    Reqs/sec     24006.13    7223.84   75516.41
    Latency        2.08ms     1.86ms   162.12ms
    HTTP codes:
      1xx - 0, 2xx - 95628, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4372
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4372
    Throughput:     5.26MB/s
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
    Reqs/sec     70987.49    3560.06   80125.96
    Latency      700.81us   175.77us    10.18ms
    HTTP codes:
      1xx - 0, 2xx - 96588, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3412
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3412
    Throughput:    10.85MB/s
  ```


