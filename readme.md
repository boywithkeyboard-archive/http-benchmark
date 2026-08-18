## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `69233` | `4437` | `84448` |
| **82%** | [Hyper Express](#hyper-express) | `56456` | `3180` | `60986` |
| **29%** | [Fastify](#fastify) | `20391` | `5765` | `36044` |
| **28%** | [Node (Default)](#node-default) | `19481` | `5285` | `66413` |
| **27%** | [Hono](#hono) | `18466` | `5864` | `31486` |
| **27%** | [Koa](#koa) | `18351` | `7748` | `56787` |
| **11%** | [Carbon](#carbon) | `7290` | `1252` | `10306` |
| **9%** | [Express](#express) | `6133` | `1098` | `8142` |


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
    Reqs/sec      7699.97    5768.44   77088.20
    Latency        6.48ms     4.68ms   395.69ms
    HTTP codes:
      1xx - 0, 2xx - 90825, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9175
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9175
    Throughput:     1.59MB/s
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
    Reqs/sec      6093.20    1088.62    8246.00
    Latency        8.20ms     3.90ms   371.91ms
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
    Reqs/sec     19757.48    5235.12   36262.80
    Latency        2.53ms     2.20ms   195.68ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.48MB/s
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
    Reqs/sec     20191.46    5943.68   29803.29
    Latency        2.47ms     2.22ms   198.59ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.56MB/s
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
    Reqs/sec     57986.93    3199.00   65262.58
    Latency        0.86ms    96.04us     4.48ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.23MB/s
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
    Reqs/sec     19147.38    9629.19   74242.12
    Latency        2.61ms     2.55ms   220.41ms
    HTTP codes:
      1xx - 0, 2xx - 90226, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9774
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9774
    Throughput:     3.91MB/s
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
    Reqs/sec     19070.22    4625.39   63267.67
    Latency        2.62ms     1.94ms   169.71ms
    HTTP codes:
      1xx - 0, 2xx - 97356, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2644
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2644
    Throughput:     4.25MB/s
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
    Reqs/sec     70323.31    4602.16   84190.61
    Latency      707.42us   201.34us    12.96ms
    HTTP codes:
      1xx - 0, 2xx - 96507, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3493
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3493
    Throughput:    10.74MB/s
  ```


