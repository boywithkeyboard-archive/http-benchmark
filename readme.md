## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73016` | `4528` | `78214` |
| **85%** | [Hyper Express](#hyper-express) | `61916` | `3185` | `79854` |
| **35%** | [Node (Default)](#node-default) | `25474` | `7289` | `49757` |
| **31%** | [Fastify](#fastify) | `22901` | `6433` | `36510` |
| **27%** | [Hono](#hono) | `19655` | `5306` | `29063` |
| **25%** | [Koa](#koa) | `18054` | `5820` | `46537` |
| **11%** | [Carbon](#carbon) | `8223` | `1410` | `10231` |
| **9%** | [Express](#express) | `6403` | `1058` | `8402` |


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
    Reqs/sec      8674.02    4205.35   49683.98
    Latency        5.75ms     4.44ms   379.74ms
    HTTP codes:
      1xx - 0, 2xx - 93401, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6599
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6599
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
    Reqs/sec      6367.30    1053.83    8356.63
    Latency        7.85ms     3.74ms   358.63ms
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
    Reqs/sec     24396.57    7561.26   36517.33
    Latency        2.05ms     2.10ms   187.56ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.54MB/s
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
    Reqs/sec     20834.87    5855.70   29240.72
    Latency        2.40ms     2.08ms   187.83ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.70MB/s
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
    Reqs/sec     61551.70    3718.98   63978.32
    Latency      809.30us    73.57us     2.91ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.75MB/s
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
    Reqs/sec     18569.26    7540.35   60430.76
    Latency        2.69ms     2.50ms   217.25ms
    HTTP codes:
      1xx - 0, 2xx - 92570, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7430
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7430
    Throughput:     3.89MB/s
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
    Reqs/sec     26277.54    8619.42   47692.78
    Latency        1.90ms     2.12ms   180.00ms
    HTTP codes:
      1xx - 0, 2xx - 98052, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1948
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1948
    Throughput:     5.90MB/s
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
    Reqs/sec     73297.25    5134.02   80084.69
    Latency      680.65us   155.23us     6.92ms
    HTTP codes:
      1xx - 0, 2xx - 98086, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1914
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1914
    Throughput:    11.36MB/s
  ```


