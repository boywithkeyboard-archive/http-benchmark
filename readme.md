## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72950` | `5827` | `77656` |
| **85%** | [Hyper Express](#hyper-express) | `62107` | `3942` | `68723` |
| **36%** | [Node (Default)](#node-default) | `26012` | `7639` | `52098` |
| **33%** | [Fastify](#fastify) | `23875` | `7038` | `35091` |
| **26%** | [Hono](#hono) | `19141` | `5274` | `28548` |
| **25%** | [Koa](#koa) | `18024` | `6445` | `60964` |
| **12%** | [Carbon](#carbon) | `8390` | `1436` | `10599` |
| **9%** | [Express](#express) | `6569` | `1050` | `8427` |


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
    Reqs/sec      8568.78    3750.35   47009.45
    Latency        5.82ms     4.26ms   371.19ms
    HTTP codes:
      1xx - 0, 2xx - 94832, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5168
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5168
    Throughput:     1.85MB/s
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
    Reqs/sec      6592.66    1083.85    8439.20
    Latency        7.58ms     3.57ms   341.58ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     23487.27    6482.47   34936.40
    Latency        2.13ms     1.96ms   178.79ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.33MB/s
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
    Reqs/sec     20251.82    5569.70   29579.44
    Latency        2.47ms     1.91ms   171.54ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.58MB/s
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
    Reqs/sec     61734.13    3624.81   70679.34
    Latency      809.18us    64.78us     3.43ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.75MB/s
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
    Reqs/sec     18387.22    6311.02   53487.62
    Latency        2.71ms     2.39ms   210.54ms
    HTTP codes:
      1xx - 0, 2xx - 94929, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5071
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5071
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
    Reqs/sec     26155.90    8016.47   58288.28
    Latency        1.91ms     1.97ms   163.64ms
    HTTP codes:
      1xx - 0, 2xx - 96542, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3458
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3458
    Throughput:     5.77MB/s
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
    Reqs/sec     70827.63    7408.76   80861.28
    Latency      701.39us   321.02us    19.70ms
    HTTP codes:
      1xx - 0, 2xx - 96800, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3200
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3200
    Throughput:    10.84MB/s
  ```


