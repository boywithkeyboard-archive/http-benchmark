## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75703` | `2900` | `83633` |
| **83%** | [Hyper Express](#hyper-express) | `62883` | `3423` | `66730` |
| **35%** | [Node (Default)](#node-default) | `26187` | `8294` | `59541` |
| **32%** | [Fastify](#fastify) | `24232` | `7397` | `36287` |
| **29%** | [Hono](#hono) | `21734` | `6352` | `29678` |
| **25%** | [Koa](#koa) | `19014` | `8999` | `79790` |
| **11%** | [Carbon](#carbon) | `8358` | `1439` | `10315` |
| **9%** | [Express](#express) | `6530` | `1073` | `8512` |


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
    Reqs/sec      8811.65    5187.07   63734.28
    Latency        5.66ms     4.25ms   366.73ms
    HTTP codes:
      1xx - 0, 2xx - 93019, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6981
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6981
    Throughput:     1.86MB/s
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
    Reqs/sec      6977.83    5347.34   67397.74
    Latency        7.16ms     3.87ms   355.75ms
    HTTP codes:
      1xx - 0, 2xx - 91225, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8775
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8775
    Throughput:     1.82MB/s
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
    Reqs/sec     24656.60    7976.85   36202.48
    Latency        2.03ms     2.11ms   187.21ms
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
    Reqs/sec     21107.81    5881.10   30301.98
    Latency        2.37ms     1.99ms   177.62ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.77MB/s
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
    Reqs/sec     62817.88    3411.35   66146.46
    Latency      793.77us    71.07us     2.94ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.92MB/s
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
    Reqs/sec     19111.86    9065.56   83178.83
    Latency        2.61ms     2.44ms   212.78ms
    HTTP codes:
      1xx - 0, 2xx - 91445, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8555
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8555
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
    Reqs/sec     25597.78    8183.73   71009.37
    Latency        1.95ms     1.92ms   163.94ms
    HTTP codes:
      1xx - 0, 2xx - 96449, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3551
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3551
    Throughput:     5.66MB/s
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
    Reqs/sec     74996.00    2371.66   81940.97
    Latency      664.26us   139.30us     6.83ms
    HTTP codes:
      1xx - 0, 2xx - 97191, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2809
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2809
    Throughput:    11.54MB/s
  ```


