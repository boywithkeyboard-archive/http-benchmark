## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74276` | `5054` | `82222` |
| **85%** | [Hyper Express](#hyper-express) | `63478` | `4061` | `69519` |
| **34%** | [Node (Default)](#node-default) | `25610` | `6988` | `48181` |
| **32%** | [Fastify](#fastify) | `24051` | `7353` | `37499` |
| **30%** | [Hono](#hono) | `21936` | `6059` | `30063` |
| **27%** | [Koa](#koa) | `19739` | `7339` | `56787` |
| **11%** | [Carbon](#carbon) | `8374` | `1450` | `10536` |
| **9%** | [Express](#express) | `6560` | `1057` | `8577` |


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
    Reqs/sec      8992.23    4917.26   61183.55
    Latency        5.55ms     4.15ms   364.26ms
    HTTP codes:
      1xx - 0, 2xx - 93065, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6935
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6935
    Throughput:     1.90MB/s
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
    Reqs/sec      7106.75    4220.25   60346.86
    Latency        7.03ms     3.95ms   359.92ms
    HTTP codes:
      1xx - 0, 2xx - 92746, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7254
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7254
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
    Reqs/sec     25168.17    7703.34   37034.84
    Latency        1.98ms     1.94ms   175.44ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.71MB/s
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
    Reqs/sec     22180.42    6016.95   32036.66
    Latency        2.25ms     1.94ms   174.06ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.01MB/s
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
    Reqs/sec     62619.65    4114.89   71652.99
    Latency      796.08us    78.29us     3.10ms
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
    Reqs/sec     18537.74    6305.90   50824.36
    Latency        2.69ms     2.30ms   200.83ms
    HTTP codes:
      1xx - 0, 2xx - 94709, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5291
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5291
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
    Reqs/sec     26772.93    8129.12   55332.62
    Latency        1.86ms     1.91ms   162.28ms
    HTTP codes:
      1xx - 0, 2xx - 96971, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3029
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3029
    Throughput:     5.94MB/s
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
    Reqs/sec     73535.82    5655.27   96521.19
    Latency      676.65us   166.91us     7.47ms
    HTTP codes:
      1xx - 0, 2xx - 96956, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3044
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3044
    Throughput:    11.29MB/s
  ```


