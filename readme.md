## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74133` | `2978` | `81688` |
| **86%** | [Hyper Express](#hyper-express) | `63573` | `3825` | `68783` |
| **35%** | [Node (Default)](#node-default) | `25828` | `7576` | `60903` |
| **33%** | [Fastify](#fastify) | `24394` | `7680` | `36352` |
| **29%** | [Hono](#hono) | `21196` | `6087` | `30310` |
| **26%** | [Koa](#koa) | `18959` | `8194` | `68974` |
| **12%** | [Carbon](#carbon) | `8579` | `1521` | `10556` |
| **9%** | [Express](#express) | `6541` | `1090` | `8437` |


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
    Reqs/sec      8856.97    6241.73   76169.15
    Latency        5.63ms     4.29ms   369.79ms
    HTTP codes:
      1xx - 0, 2xx - 91178, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8822
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8822
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
    Reqs/sec      7341.95    6831.51   77132.24
    Latency        6.80ms     3.89ms   358.22ms
    HTTP codes:
      1xx - 0, 2xx - 88172, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11828
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11828
    Throughput:     1.85MB/s
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
    Reqs/sec     23798.91    7420.48   36035.39
    Latency        2.10ms     2.07ms   182.13ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.40MB/s
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
    Reqs/sec     21093.60    5886.69   29618.01
    Latency        2.37ms     2.07ms   184.46ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.76MB/s
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
    Reqs/sec     63184.63    3916.70   66386.84
    Latency      788.49us    79.91us     3.39ms
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
    Reqs/sec     19187.65    8638.62   74930.24
    Latency        2.60ms     2.38ms   208.47ms
    HTTP codes:
      1xx - 0, 2xx - 92094, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7906
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7906
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
    Reqs/sec     24961.17    8454.23   82564.61
    Latency        2.00ms     1.94ms   162.26ms
    HTTP codes:
      1xx - 0, 2xx - 96512, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3488
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3488
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
    Reqs/sec     75940.07    2937.48   84617.24
    Latency      655.60us   132.59us     5.56ms
    HTTP codes:
      1xx - 0, 2xx - 96713, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3287
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3287
    Throughput:    11.62MB/s
  ```


