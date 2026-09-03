## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `79516` | `2274` | `87231` |
| **87%** | [Hyper Express](#hyper-express) | `69313` | `3034` | `72970` |
| **46%** | [Node (Default)](#node-default) | `36428` | `10728` | `83935` |
| **42%** | [Fastify](#fastify) | `33564` | `9564` | `51848` |
| **39%** | [Koa](#koa) | `31014` | `13053` | `74949` |
| **37%** | [Hono](#hono) | `29805` | `8780` | `47225` |
| **12%** | [Carbon](#carbon) | `9484` | `2358` | `13611` |
| **10%** | [Express](#express) | `7697` | `1772` | `10831` |


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
    Reqs/sec     10258.00    7913.01   91060.68
    Latency        4.86ms     4.24ms   362.37ms
    HTTP codes:
      1xx - 0, 2xx - 89346, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10654
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10654
    Throughput:     2.08MB/s
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
    Reqs/sec      8108.10    6935.61   81671.16
    Latency        6.15ms     3.70ms   337.69ms
    HTTP codes:
      1xx - 0, 2xx - 89525, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10475
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10458
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 17
    Throughput:     2.08MB/s
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
    Reqs/sec     33693.67    9166.06   51853.28
    Latency        1.48ms     1.97ms   169.81ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.64MB/s
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
    Reqs/sec     28887.63    7959.92   46139.11
    Latency        1.73ms     2.13ms   184.34ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     6.53MB/s
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
    Reqs/sec     69501.58    2986.23   72938.34
    Latency      717.56us    75.84us     2.82ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.87MB/s
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
    Reqs/sec     31774.30   12705.44   73637.97
    Latency        1.57ms     2.28ms   195.47ms
    HTTP codes:
      1xx - 0, 2xx - 92968, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7032
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7032
    Throughput:     6.67MB/s
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
    Reqs/sec     35623.02   10738.47   86642.58
    Latency        1.40ms     1.72ms   142.54ms
    HTTP codes:
      1xx - 0, 2xx - 94342, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5658
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5658
    Throughput:     7.69MB/s
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
    Reqs/sec     75016.17    6964.10   84976.77
    Latency      663.12us   235.61us    10.99ms
    HTTP codes:
      1xx - 0, 2xx - 95690, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4310
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4310
    Throughput:    11.37MB/s
  ```


