## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `69048` | `3406` | `77420` |
| **84%** | [Hyper Express](#hyper-express) | `57878` | `3223` | `63872` |
| **30%** | [Fastify](#fastify) | `20859` | `6305` | `36136` |
| **29%** | [Hono](#hono) | `20046` | `5951` | `29615` |
| **29%** | [Node (Default)](#node-default) | `19974` | `6111` | `78232` |
| **28%** | [Koa](#koa) | `19321` | `9243` | `75132` |
| **10%** | [Carbon](#carbon) | `7093` | `1081` | `10195` |
| **9%** | [Express](#express) | `6050` | `1122` | `8124` |


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
    Reqs/sec      7734.12    5138.23   62346.85
    Latency        6.45ms     4.75ms   401.82ms
    HTTP codes:
      1xx - 0, 2xx - 92172, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7828
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7828
    Throughput:     1.62MB/s
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
    Reqs/sec      6075.99    1147.01    8152.77
    Latency        8.23ms     3.97ms   378.62ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.74MB/s
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
    Reqs/sec     19197.00    4229.55   35531.20
    Latency        2.61ms     2.12ms   191.56ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.34MB/s
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
    Reqs/sec     22247.31    7001.34   30609.05
    Latency        2.25ms     2.27ms   200.40ms
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
    Reqs/sec     58073.27    3545.89   64198.61
    Latency        0.86ms    98.41us     3.43ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.24MB/s
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
    Reqs/sec     18564.71    8625.99   68239.70
    Latency        2.69ms     2.57ms   226.84ms
    HTTP codes:
      1xx - 0, 2xx - 91639, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8361
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8361
    Throughput:     3.85MB/s
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
    Reqs/sec     19777.70    5857.22   73613.46
    Latency        2.52ms     1.94ms   170.88ms
    HTTP codes:
      1xx - 0, 2xx - 95967, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4033
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4033
    Throughput:     4.35MB/s
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
    Reqs/sec     68644.87    4522.42   82982.11
    Latency      725.56us   164.60us     8.58ms
    HTTP codes:
      1xx - 0, 2xx - 97041, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2959
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2959
    Throughput:    10.54MB/s
  ```


