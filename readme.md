## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `71881` | `4024` | `84871` |
| **85%** | [Hyper Express](#hyper-express) | `60757` | `3555` | `67139` |
| **29%** | [Hono](#hono) | `21128` | `6506` | `31217` |
| **29%** | [Fastify](#fastify) | `21072` | `5423` | `36880` |
| **28%** | [Node (Default)](#node-default) | `20391` | `5231` | `60605` |
| **25%** | [Koa](#koa) | `17616` | `8545` | `74806` |
| **11%** | [Carbon](#carbon) | `7789` | `1391` | `10652` |
| **9%** | [Express](#express) | `6385` | `1138` | `12229` |


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
    Reqs/sec      8451.16    5501.17   67273.36
    Latency        5.90ms     4.39ms   376.08ms
    HTTP codes:
      1xx - 0, 2xx - 92297, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7703
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7703
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
    Reqs/sec      6454.98    1144.52    8747.18
    Latency        7.74ms     3.70ms   355.99ms
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
    Reqs/sec     22018.50    6096.10   36804.91
    Latency        2.27ms     2.06ms   183.49ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.99MB/s
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
    Reqs/sec     20776.93    6315.73   30942.43
    Latency        2.41ms     2.31ms   197.35ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.69MB/s
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
    Reqs/sec     60238.09    3710.50   65679.46
    Latency      827.59us    98.18us     4.46ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.56MB/s
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
    Reqs/sec     18873.90    8413.21   68514.12
    Latency        2.64ms     2.17ms   195.04ms
    HTTP codes:
      1xx - 0, 2xx - 93066, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6934
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6934
    Throughput:     3.97MB/s
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
    Reqs/sec     21737.33    5542.11   60372.15
    Latency        2.29ms     1.96ms   167.58ms
    HTTP codes:
      1xx - 0, 2xx - 97342, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2658
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2658
    Throughput:     4.85MB/s
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
    Reqs/sec     71586.42    4341.66   84091.65
    Latency      696.28us   158.68us     5.79ms
    HTTP codes:
      1xx - 0, 2xx - 97361, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2639
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2639
    Throughput:    11.03MB/s
  ```


