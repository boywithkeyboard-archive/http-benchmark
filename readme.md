## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73572` | `4948` | `83479` |
| **85%** | [Hyper Express](#hyper-express) | `62760` | `3019` | `68448` |
| **34%** | [Node (Default)](#node-default) | `24815` | `7096` | `50826` |
| **32%** | [Fastify](#fastify) | `23601` | `6668` | `35779` |
| **28%** | [Hono](#hono) | `20251` | `5460` | `29261` |
| **26%** | [Koa](#koa) | `19059` | `7476` | `54665` |
| **11%** | [Carbon](#carbon) | `8188` | `1415` | `10264` |
| **9%** | [Express](#express) | `6433` | `1058` | `8379` |


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
    Reqs/sec      8924.20    5017.23   58782.25
    Latency        5.59ms     4.30ms   369.95ms
    HTTP codes:
      1xx - 0, 2xx - 92171, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7829
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7829
    Throughput:     1.87MB/s
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
    Reqs/sec      7005.43    4674.07   54932.17
    Latency        7.13ms     3.92ms   357.94ms
    HTTP codes:
      1xx - 0, 2xx - 91027, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8973
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8973
    Throughput:     1.83MB/s
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
    Reqs/sec     24318.08    7773.01   36107.23
    Latency        2.06ms     2.11ms   186.44ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.51MB/s
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
    Reqs/sec     19830.66    5129.09   27946.77
    Latency        2.52ms     2.05ms   182.60ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.48MB/s
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
    Reqs/sec     61242.73    4335.39   65523.73
    Latency      814.40us   101.51us     3.72ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.70MB/s
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
    Reqs/sec     18896.72    7166.31   62061.63
    Latency        2.64ms     2.43ms   214.34ms
    HTTP codes:
      1xx - 0, 2xx - 93602, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6398
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6398
    Throughput:     4.00MB/s
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
    Reqs/sec     25784.90    8511.33   59128.39
    Latency        1.94ms     2.02ms   169.65ms
    HTTP codes:
      1xx - 0, 2xx - 96719, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3281
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3281
    Throughput:     5.70MB/s
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
    Reqs/sec     73370.25    5271.37   84284.88
    Latency      678.03us   158.95us     6.40ms
    HTTP codes:
      1xx - 0, 2xx - 97474, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2526
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2526
    Throughput:    11.32MB/s
  ```


