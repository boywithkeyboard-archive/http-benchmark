## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73417` | `2704` | `78935` |
| **85%** | [Hyper Express](#hyper-express) | `62113` | `3582` | `65520` |
| **35%** | [Node (Default)](#node-default) | `25731` | `7998` | `62620` |
| **34%** | [Fastify](#fastify) | `24835` | `7718` | `36610` |
| **29%** | [Hono](#hono) | `21409` | `6155` | `30475` |
| **28%** | [Koa](#koa) | `20255` | `9307` | `78972` |
| **11%** | [Carbon](#carbon) | `8172` | `1430` | `10387` |
| **9%** | [Express](#express) | `6525` | `1126` | `8338` |


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
    Reqs/sec      9264.00    7352.39   77586.26
    Latency        5.39ms     4.26ms   366.53ms
    HTTP codes:
      1xx - 0, 2xx - 88998, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11002
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11002
    Throughput:     1.87MB/s
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
    Reqs/sec      7089.44    5378.76   67464.39
    Latency        7.03ms     3.93ms   357.17ms
    HTTP codes:
      1xx - 0, 2xx - 91169, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8831
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8831
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
    Reqs/sec     23841.49    7593.25   36104.69
    Latency        2.10ms     1.99ms   180.59ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.41MB/s
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
    Reqs/sec     22301.18    6262.42   30388.40
    Latency        2.24ms     1.98ms   178.34ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.04MB/s
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
    Reqs/sec     61775.22    3659.58   75631.38
    Latency      807.01us    83.22us     2.94ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.78MB/s
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
    Reqs/sec     18685.82    8519.13   72983.68
    Latency        2.67ms     2.41ms   210.51ms
    HTTP codes:
      1xx - 0, 2xx - 92159, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7841
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7841
    Throughput:     3.90MB/s
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
    Reqs/sec     24701.51    8071.84   79473.22
    Latency        2.02ms     1.93ms   166.53ms
    HTTP codes:
      1xx - 0, 2xx - 95611, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4389
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4389
    Throughput:     5.41MB/s
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
    Reqs/sec     73513.91    2905.12   80923.83
    Latency      677.55us   154.78us     6.63ms
    HTTP codes:
      1xx - 0, 2xx - 96864, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3136
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3136
    Throughput:    11.27MB/s
  ```


