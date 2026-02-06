## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `72503` | `2366` | `77806` |
| **82%** | [Hyper Express](#hyper-express) | `59723` | `3760` | `62349` |
| **34%** | [Node (Default)](#node-default) | `24874` | `8774` | `62091` |
| **32%** | [Fastify](#fastify) | `23512` | `7974` | `36540` |
| **28%** | [Hono](#hono) | `20039` | `6219` | `31369` |
| **25%** | [Koa](#koa) | `18083` | `8104` | `65775` |
| **11%** | [Carbon](#carbon) | `7921` | `1467` | `10388` |
| **8%** | [Express](#express) | `6021` | `1025` | `8326` |


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
    Reqs/sec      8508.11    6523.00   67611.50
    Latency        5.86ms     4.54ms   392.71ms
    HTTP codes:
      1xx - 0, 2xx - 89825, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10175
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10175
    Throughput:     1.74MB/s
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
    Reqs/sec      6050.25    1063.07    8218.50
    Latency        8.26ms     3.57ms   349.47ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.73MB/s
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
    Reqs/sec     23070.74    7518.60   37569.42
    Latency        2.17ms     1.96ms   181.26ms
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
    Reqs/sec     20178.53    6380.33   30416.47
    Latency        2.48ms     2.10ms   189.14ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.55MB/s
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
    Reqs/sec     61005.34    3484.55   63503.93
    Latency      817.37us    70.71us     3.17ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.66MB/s
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
    Reqs/sec     17779.34    6516.59   51381.44
    Latency        2.80ms     2.60ms   220.42ms
    HTTP codes:
      1xx - 0, 2xx - 95200, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4800
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4800
    Throughput:     3.83MB/s
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
    Reqs/sec     23921.26    7920.13   64959.26
    Latency        2.09ms     1.93ms   165.19ms
    HTTP codes:
      1xx - 0, 2xx - 97169, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2831
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2831
    Throughput:     5.32MB/s
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
    Reqs/sec     73251.04    3240.02   87546.78
    Latency      679.61us   187.54us    12.56ms
    HTTP codes:
      1xx - 0, 2xx - 96016, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3984
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3984
    Throughput:    11.13MB/s
  ```


