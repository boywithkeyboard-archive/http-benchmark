## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74725` | `2784` | `89076` |
| **85%** | [Hyper Express](#hyper-express) | `63299` | `4231` | `67146` |
| **35%** | [Node (Default)](#node-default) | `26285` | `8608` | `65327` |
| **32%** | [Fastify](#fastify) | `24164` | `7446` | `36654` |
| **28%** | [Hono](#hono) | `20581` | `5309` | `29920` |
| **26%** | [Koa](#koa) | `19128` | `8413` | `72822` |
| **11%** | [Carbon](#carbon) | `8397` | `1481` | `10389` |
| **9%** | [Express](#express) | `6523` | `1098` | `8482` |


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
    Reqs/sec      8860.61    5989.99   68432.47
    Latency        5.63ms     4.23ms   367.81ms
    HTTP codes:
      1xx - 0, 2xx - 91033, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8967
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8967
    Throughput:     1.83MB/s
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
    Reqs/sec      6948.84    5647.74   77731.37
    Latency        7.19ms     3.91ms   359.43ms
    HTTP codes:
      1xx - 0, 2xx - 90651, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9349
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9349
    Throughput:     1.80MB/s
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
    Reqs/sec     24896.12    7664.15   36377.55
    Latency        2.01ms     2.04ms   184.99ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.65MB/s
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
    Reqs/sec     21695.34    6128.63   31052.22
    Latency        2.30ms     2.04ms   183.10ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.90MB/s
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
    Reqs/sec     63130.31    3657.19   68551.86
    Latency      789.14us    77.96us     3.52ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.98MB/s
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
    Reqs/sec     18535.60    7868.65   66518.40
    Latency        2.69ms     2.39ms   207.78ms
    HTTP codes:
      1xx - 0, 2xx - 92888, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7112
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7112
    Throughput:     3.89MB/s
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
    Reqs/sec     26144.48    8514.76   76555.56
    Latency        1.91ms     2.02ms   169.11ms
    HTTP codes:
      1xx - 0, 2xx - 96467, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3533
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3533
    Throughput:     5.78MB/s
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
    Reqs/sec     75755.32    2850.54   83107.10
    Latency      657.25us   109.54us     4.95ms
    HTTP codes:
      1xx - 0, 2xx - 97276, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2724
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2724
    Throughput:    11.66MB/s
  ```


