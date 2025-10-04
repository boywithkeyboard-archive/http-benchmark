## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74150` | `3121` | `84954` |
| **81%** | [Hyper Express](#hyper-express) | `60253` | `3883` | `69697` |
| **34%** | [Node (Default)](#node-default) | `25464` | `8342` | `61849` |
| **30%** | [Fastify](#fastify) | `22154` | `6259` | `34547` |
| **28%** | [Hono](#hono) | `20789` | `5979` | `29025` |
| **25%** | [Koa](#koa) | `18594` | `8605` | `79243` |
| **11%** | [Carbon](#carbon) | `7974` | `1456` | `10275` |
| **8%** | [Express](#express) | `6085` | `1027` | `8230` |


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
    Reqs/sec      8602.43    5743.42   74546.44
    Latency        5.80ms     4.39ms   378.72ms
    HTTP codes:
      1xx - 0, 2xx - 91875, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8125
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8125
    Throughput:     1.80MB/s
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
    Reqs/sec      6079.21    1052.05    8115.69
    Latency        8.22ms     3.88ms   369.77ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.74MB/s
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
    Reqs/sec     23055.08    7201.88   34464.72
    Latency        2.17ms     2.15ms   191.03ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.23MB/s
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
    Reqs/sec     19614.32    5307.65   27846.10
    Latency        2.55ms     2.07ms   186.02ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.43MB/s
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
    Reqs/sec     60990.93    3002.67   65013.27
    Latency      817.65us    70.88us     2.91ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.67MB/s
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
    Reqs/sec     18600.15    9669.35   80492.82
    Latency        2.68ms     2.67ms   230.22ms
    HTTP codes:
      1xx - 0, 2xx - 90047, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9953
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9953
    Throughput:     3.79MB/s
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
    Reqs/sec     24639.95    7500.40   62987.60
    Latency        2.03ms     1.90ms   162.00ms
    HTTP codes:
      1xx - 0, 2xx - 96860, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3140
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3140
    Throughput:     5.46MB/s
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
    Reqs/sec     73899.94    3279.63   81249.24
    Latency      673.81us   122.76us     4.98ms
    HTTP codes:
      1xx - 0, 2xx - 97458, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2542
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2542
    Throughput:    11.40MB/s
  ```


