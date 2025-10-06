## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75664` | `3522` | `87016` |
| **82%** | [Hyper Express](#hyper-express) | `62196` | `3744` | `65994` |
| **34%** | [Node (Default)](#node-default) | `25719` | `7839` | `59661` |
| **32%** | [Fastify](#fastify) | `24276` | `7317` | `37109` |
| **27%** | [Hono](#hono) | `20415` | `6315` | `30130` |
| **25%** | [Koa](#koa) | `18888` | `8351` | `75481` |
| **11%** | [Carbon](#carbon) | `8140` | `1410` | `10450` |
| **9%** | [Express](#express) | `6545` | `1126` | `8445` |


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
    Reqs/sec      8896.90    6526.35   82484.95
    Latency        5.60ms     4.35ms   375.48ms
    HTTP codes:
      1xx - 0, 2xx - 90785, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9215
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9215
    Throughput:     1.84MB/s
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
    Reqs/sec      6928.63    6351.49   83481.34
    Latency        7.20ms     3.88ms   356.20ms
    HTTP codes:
      1xx - 0, 2xx - 89031, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10969
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10969
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
    Reqs/sec     23788.10    7314.66   35234.64
    Latency        2.10ms     2.02ms   182.41ms
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
    Reqs/sec     21147.46    6058.23   30277.83
    Latency        2.36ms     2.06ms   186.44ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.78MB/s
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
    Reqs/sec     61842.03    3200.65   66142.42
    Latency      806.03us    75.75us     2.96ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.79MB/s
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
    Reqs/sec     18838.79    8226.35   68401.52
    Latency        2.65ms     2.24ms   197.28ms
    HTTP codes:
      1xx - 0, 2xx - 92840, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7160
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7160
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
    Reqs/sec     26061.06    8449.73   68391.02
    Latency        1.91ms     2.00ms   171.22ms
    HTTP codes:
      1xx - 0, 2xx - 96847, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3153
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3153
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
    Reqs/sec     76538.40    2940.16   84079.69
    Latency      649.93us   169.75us     9.53ms
    HTTP codes:
      1xx - 0, 2xx - 96352, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3648
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3648
    Throughput:    11.67MB/s
  ```


