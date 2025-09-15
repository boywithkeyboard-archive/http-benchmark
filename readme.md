## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73344` | `2959` | `82236` |
| **83%** | [Hyper Express](#hyper-express) | `60512` | `3459` | `64447` |
| **34%** | [Node (Default)](#node-default) | `24995` | `8182` | `76312` |
| **33%** | [Fastify](#fastify) | `24430` | `7950` | `37342` |
| **27%** | [Hono](#hono) | `20004` | `5569` | `29131` |
| **26%** | [Koa](#koa) | `19325` | `9098` | `70052` |
| **11%** | [Carbon](#carbon) | `8307` | `1490` | `10521` |
| **9%** | [Express](#express) | `6323` | `1086` | `8211` |


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
    Reqs/sec      8746.98    5996.41   81454.07
    Latency        5.71ms     4.30ms   371.43ms
    HTTP codes:
      1xx - 0, 2xx - 91729, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8271
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8271
    Throughput:     1.82MB/s
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
    Reqs/sec      6194.56     990.27    8270.90
    Latency        8.07ms     3.69ms   355.46ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.77MB/s
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
    Reqs/sec     23009.96    7187.72   35225.45
    Latency        2.17ms     2.11ms   187.79ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.22MB/s
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
    Reqs/sec     20922.68    6199.47   29915.58
    Latency        2.39ms     2.12ms   188.19ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.73MB/s
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
    Reqs/sec     61313.49    3512.91   65314.61
    Latency      812.98us    74.21us     4.03ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.71MB/s
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
    Reqs/sec     17856.88    8470.67   73118.77
    Latency        2.79ms     2.45ms   215.46ms
    HTTP codes:
      1xx - 0, 2xx - 91984, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8016
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8016
    Throughput:     3.72MB/s
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
    Reqs/sec     24822.66    7765.40   70932.63
    Latency        2.01ms     1.90ms   165.07ms
    HTTP codes:
      1xx - 0, 2xx - 97035, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2965
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2965
    Throughput:     5.52MB/s
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
    Reqs/sec     74616.96    3505.26   90449.40
    Latency      669.49us   136.54us     5.32ms
    HTTP codes:
      1xx - 0, 2xx - 97347, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2653
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2653
    Throughput:    11.46MB/s
  ```


