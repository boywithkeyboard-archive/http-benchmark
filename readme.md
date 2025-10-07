## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74655` | `2791` | `84256` |
| **84%** | [Hyper Express](#hyper-express) | `62528` | `5306` | `78506` |
| **35%** | [Node (Default)](#node-default) | `25926` | `7868` | `80909` |
| **34%** | [Fastify](#fastify) | `25046` | `8043` | `37502` |
| **28%** | [Hono](#hono) | `21061` | `6042` | `29924` |
| **26%** | [Koa](#koa) | `19643` | `10106` | `81403` |
| **11%** | [Carbon](#carbon) | `8322` | `1431` | `10360` |
| **9%** | [Express](#express) | `6400` | `1054` | `8270` |


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
    Reqs/sec      9346.13    7163.17   74969.68
    Latency        5.34ms     4.30ms   372.66ms
    HTTP codes:
      1xx - 0, 2xx - 88957, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11043
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11043
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
    Reqs/sec      6776.39    5346.99   66100.71
    Latency        7.37ms     4.02ms   368.89ms
    HTTP codes:
      1xx - 0, 2xx - 91517, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8483
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8483
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
    Reqs/sec     24740.09    7462.22   36568.40
    Latency        2.02ms     1.99ms   177.00ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.62MB/s
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
    Reqs/sec     20848.07    5529.50   30186.78
    Latency        2.40ms     2.02ms   181.85ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.71MB/s
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
    Reqs/sec     62333.35    3492.14   64832.90
    Latency      799.08us    70.88us     2.61ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.86MB/s
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
    Reqs/sec     19464.64    8300.07   78401.78
    Latency        2.56ms     2.37ms   207.49ms
    HTTP codes:
      1xx - 0, 2xx - 92723, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7277
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7277
    Throughput:     4.08MB/s
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
    Reqs/sec     26018.57    7466.09   58692.09
    Latency        1.92ms     1.85ms   157.66ms
    HTTP codes:
      1xx - 0, 2xx - 97666, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2334
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2334
    Throughput:     5.83MB/s
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
    Reqs/sec     75894.27    3234.22   92516.00
    Latency      658.06us   146.86us     5.83ms
    HTTP codes:
      1xx - 0, 2xx - 97103, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2897
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2897
    Throughput:    11.63MB/s
  ```


