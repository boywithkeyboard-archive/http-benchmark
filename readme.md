## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74765` | `6004` | `82773` |
| **85%** | [Hyper Express](#hyper-express) | `63835` | `4112` | `70539` |
| **36%** | [Node (Default)](#node-default) | `26936` | `8270` | `41175` |
| **31%** | [Fastify](#fastify) | `23276` | `7032` | `36670` |
| **28%** | [Hono](#hono) | `21017` | `6031` | `30698` |
| **24%** | [Koa](#koa) | `18066` | `6301` | `51846` |
| **11%** | [Carbon](#carbon) | `8067` | `1318` | `10421` |
| **9%** | [Express](#express) | `6512` | `995` | `8523` |


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
    Reqs/sec      8448.28    3400.47   43616.54
    Latency        5.91ms     4.13ms   361.32ms
    HTTP codes:
      1xx - 0, 2xx - 95561, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4439
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4439
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
    Reqs/sec      6576.01    1062.28    8580.26
    Latency        7.60ms     3.53ms   337.05ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.88MB/s
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
    Reqs/sec     24434.03    7674.59   36719.54
    Latency        2.04ms     1.86ms   170.10ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.55MB/s
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
    Reqs/sec     20871.07    5635.17   30415.21
    Latency        2.39ms     1.95ms   177.85ms
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
    Reqs/sec     64631.48    4214.24   70886.83
    Latency      771.32us    72.80us     4.04ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.18MB/s
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
    Reqs/sec     18363.24    7850.73   74844.42
    Latency        2.71ms     2.46ms   211.76ms
    HTTP codes:
      1xx - 0, 2xx - 91159, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8841
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8841
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
    Reqs/sec     26045.94    7749.50   46392.74
    Latency        1.92ms     1.93ms   164.91ms
    HTTP codes:
      1xx - 0, 2xx - 98149, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1851
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1851
    Throughput:     5.85MB/s
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
    Reqs/sec     76324.84    7537.87   87444.86
    Latency      653.77us   243.50us    13.36ms
    HTTP codes:
      1xx - 0, 2xx - 98084, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1916
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1916
    Throughput:    11.82MB/s
  ```


