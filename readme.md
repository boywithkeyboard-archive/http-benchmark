## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `125791` | `9450` | `169452` |
| **79%** | [Hyper Express](#hyper-express) | `99295` | `7697` | `106267` |
| **29%** | [Node (Default)](#node-default) | `36769` | `12369` | `118481` |
| **28%** | [Fastify](#fastify) | `35146` | `8615` | `56568` |
| **25%** | [Hono](#hono) | `31865` | `7740` | `48719` |
| **25%** | [Koa](#koa) | `31757` | `12430` | `88770` |
| **9%** | [Carbon](#carbon) | `11656` | `2306` | `16995` |
| **7%** | [Express](#express) | `8771` | `1717` | `11981` |


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
    Reqs/sec     14237.15   13121.95  115642.04
    Latency        3.50ms     3.72ms   319.14ms
    HTTP codes:
      1xx - 0, 2xx - 85323, 3xx - 0, 4xx - 0, 5xx - 0
      others - 14677
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 14677
    Throughput:     2.76MB/s
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
    Reqs/sec      9858.91   10132.29  111434.37
    Latency        5.06ms     3.28ms   306.91ms
    HTTP codes:
      1xx - 0, 2xx - 87028, 3xx - 0, 4xx - 0, 5xx - 0
      others - 12972
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 12967
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 5
    Throughput:     2.46MB/s
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
    Reqs/sec     36812.07   10370.07   57121.02
    Latency        1.36ms     1.32ms   121.33ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.35MB/s
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
    Reqs/sec     32165.59    6812.89   48494.73
    Latency        1.55ms     1.54ms   134.02ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     7.27MB/s
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
    Reqs/sec     98950.61    7212.57  103887.91
    Latency      503.68us    52.90us     2.75ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:    14.05MB/s
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
    Reqs/sec     32249.28   15372.32  111644.87
    Latency        1.55ms     1.74ms   154.44ms
    HTTP codes:
      1xx - 0, 2xx - 89266, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10734
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10734
    Throughput:     6.51MB/s
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
    Reqs/sec     37799.92   12768.87  118110.43
    Latency        1.32ms     1.36ms   114.07ms
    HTTP codes:
      1xx - 0, 2xx - 94520, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5480
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5480
    Throughput:     8.19MB/s
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
    Reqs/sec    121980.02    6425.48  139463.72
    Latency      407.11us   151.66us     7.41ms
    HTTP codes:
      1xx - 0, 2xx - 92424, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7576
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7576
    Throughput:    17.84MB/s
  ```


