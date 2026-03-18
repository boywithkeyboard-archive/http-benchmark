## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `70073` | `3599` | `80847` |
| **85%** | [Hyper Express](#hyper-express) | `59370` | `3883` | `66237` |
| **32%** | [Node (Default)](#node-default) | `22176` | `5789` | `68878` |
| **31%** | [Hono](#hono) | `21839` | `7138` | `31554` |
| **30%** | [Fastify](#fastify) | `20879` | `5145` | `37268` |
| **27%** | [Koa](#koa) | `18571` | `8606` | `72501` |
| **11%** | [Carbon](#carbon) | `7931` | `1523` | `10430` |
| **9%** | [Express](#express) | `6166` | `1077` | `8378` |


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
    Reqs/sec      8603.99    5808.03   65921.11
    Latency        5.80ms     4.33ms   373.16ms
    HTTP codes:
      1xx - 0, 2xx - 91520, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8480
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8480
    Throughput:     1.79MB/s
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
    Reqs/sec      6177.90    1127.98    8443.56
    Latency        8.09ms     3.72ms   360.53ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     21118.64    5464.86   36343.28
    Latency        2.37ms     2.15ms   193.89ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.79MB/s
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
    Reqs/sec     21040.63    6667.39   30562.48
    Latency        2.37ms     2.10ms   190.40ms
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
    Reqs/sec     58200.24    3302.44   67896.46
    Latency        0.86ms    80.32us     4.56ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.27MB/s
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
    Reqs/sec     17093.95    7412.58   61754.63
    Latency        2.92ms     2.38ms   209.84ms
    HTTP codes:
      1xx - 0, 2xx - 92902, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7098
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7098
    Throughput:     3.59MB/s
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
    Reqs/sec     21942.61    7001.05   71968.33
    Latency        2.27ms     2.02ms   176.71ms
    HTTP codes:
      1xx - 0, 2xx - 95697, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4303
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4303
    Throughput:     4.81MB/s
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
    Reqs/sec     69376.42    3556.08   78201.00
    Latency      718.46us   142.48us     6.73ms
    HTTP codes:
      1xx - 0, 2xx - 97592, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2408
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2408
    Throughput:    10.71MB/s
  ```


