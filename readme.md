## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `71678` | `3637` | `80192` |
| **82%** | [Hyper Express](#hyper-express) | `58883` | `3386` | `62478` |
| **33%** | [Node (Default)](#node-default) | `23727` | `7803` | `76078` |
| **32%** | [Fastify](#fastify) | `23084` | `6280` | `37885` |
| **30%** | [Hono](#hono) | `21810` | `6655` | `32575` |
| **28%** | [Koa](#koa) | `20070` | `10117` | `77514` |
| **11%** | [Carbon](#carbon) | `8106` | `1468` | `10595` |
| **9%** | [Express](#express) | `6441` | `1121` | `8514` |


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
    Reqs/sec      8808.82    5574.77   75346.03
    Latency        5.66ms     4.24ms   364.12ms
    HTTP codes:
      1xx - 0, 2xx - 92270, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7730
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7730
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
    Reqs/sec      6470.51    1159.21    8573.80
    Latency        7.72ms     3.78ms   361.19ms
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
    Reqs/sec     24086.77    7216.95   38977.38
    Latency        2.07ms     2.03ms   182.24ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.47MB/s
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
    Reqs/sec     22229.04    6687.15   31741.17
    Latency        2.25ms     2.04ms   183.03ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.02MB/s
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
    Reqs/sec     60107.41    2884.38   65863.05
    Latency      829.78us    75.24us     2.90ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.54MB/s
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
    Reqs/sec     18921.83    8796.89   73970.33
    Latency        2.64ms     2.44ms   211.20ms
    HTTP codes:
      1xx - 0, 2xx - 91463, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8537
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8537
    Throughput:     3.91MB/s
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
    Reqs/sec     24389.51    5824.60   53680.96
    Latency        2.05ms     1.89ms   162.32ms
    HTTP codes:
      1xx - 0, 2xx - 97820, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2180
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2180
    Throughput:     5.46MB/s
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
    Reqs/sec     72066.97    4115.28   82676.96
    Latency      691.39us   151.28us     6.25ms
    HTTP codes:
      1xx - 0, 2xx - 97105, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2895
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2895
    Throughput:    11.07MB/s
  ```


