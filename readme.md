## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74251` | `2202` | `82184` |
| **84%** | [Hyper Express](#hyper-express) | `62110` | `3538` | `65062` |
| **34%** | [Node (Default)](#node-default) | `25089` | `8473` | `72550` |
| **30%** | [Fastify](#fastify) | `22384` | `6830` | `35466` |
| **27%** | [Hono](#hono) | `19818` | `5634` | `29427` |
| **25%** | [Koa](#koa) | `18690` | `8702` | `75184` |
| **11%** | [Carbon](#carbon) | `8145` | `1435` | `10240` |
| **9%** | [Express](#express) | `6370` | `1071` | `8331` |


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
    Reqs/sec      8706.34    5757.56   70066.48
    Latency        5.73ms     4.34ms   375.82ms
    HTTP codes:
      1xx - 0, 2xx - 91788, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8212
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8212
    Throughput:     1.82MB/s
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
    Reqs/sec      6370.17    1056.58    8255.50
    Latency        7.85ms     3.65ms   352.76ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     23001.21    7197.97   35456.73
    Latency        2.17ms     2.11ms   190.67ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.22MB/s
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
    Reqs/sec     20215.50    5812.08   29316.48
    Latency        2.47ms     2.05ms   186.79ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.57MB/s
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
    Reqs/sec     61897.44    3328.21   64693.58
    Latency      805.81us    68.78us     2.79ms
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
    Reqs/sec     18926.86    8704.31   75944.22
    Latency        2.63ms     2.48ms   217.50ms
    HTTP codes:
      1xx - 0, 2xx - 92149, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7851
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7851
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
    Reqs/sec     25016.80    8154.34   67558.07
    Latency        2.00ms     1.88ms   161.79ms
    HTTP codes:
      1xx - 0, 2xx - 96905, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3095
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3095
    Throughput:     5.55MB/s
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
    Reqs/sec     74885.85    3046.78   84172.94
    Latency      664.70us   157.38us     9.10ms
    HTTP codes:
      1xx - 0, 2xx - 96627, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3373
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3373
    Throughput:    11.44MB/s
  ```


