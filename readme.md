## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73537` | `4720` | `82826` |
| **84%** | [Hyper Express](#hyper-express) | `61951` | `3420` | `65151` |
| **36%** | [Node (Default)](#node-default) | `26240` | `8446` | `60693` |
| **33%** | [Fastify](#fastify) | `24227` | `7422` | `35973` |
| **27%** | [Hono](#hono) | `20178` | `5589` | `29366` |
| **25%** | [Koa](#koa) | `18049` | `6786` | `54632` |
| **11%** | [Carbon](#carbon) | `8112` | `1387` | `10282` |
| **9%** | [Express](#express) | `6331` | `1019` | `8346` |


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
    Reqs/sec      8655.95    4100.86   47342.32
    Latency        5.77ms     4.74ms   407.89ms
    HTTP codes:
      1xx - 0, 2xx - 93493, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6507
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6507
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
    Reqs/sec      6455.20    1077.63    8403.33
    Latency        7.74ms     3.72ms   359.73ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
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
    Reqs/sec     23966.12    7389.69   35685.30
    Latency        2.08ms     2.09ms   187.50ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.44MB/s
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
    Reqs/sec     20334.84    5920.28   30078.65
    Latency        2.46ms     1.98ms   178.02ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.59MB/s
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
    Reqs/sec     62183.74    3691.69   65303.08
    Latency      801.86us    71.00us     3.27ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.83MB/s
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
    Reqs/sec     18505.60    6991.04   60315.77
    Latency        2.70ms     2.44ms   216.48ms
    HTTP codes:
      1xx - 0, 2xx - 93736, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6264
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6264
    Throughput:     3.92MB/s
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
    Reqs/sec     24539.30    6843.63   47822.91
    Latency        2.03ms     1.97ms   173.46ms
    HTTP codes:
      1xx - 0, 2xx - 98050, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1950
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1950
    Throughput:     5.51MB/s
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
    Reqs/sec     73338.35    6271.74   81927.86
    Latency      678.56us   287.34us    10.46ms
    HTTP codes:
      1xx - 0, 2xx - 95759, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4241
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4241
    Throughput:    11.12MB/s
  ```


