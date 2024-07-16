## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75527` | `6455` | `85171` |
| **83%** | [Hyper Express](#hyper-express) | `62746` | `2931` | `65024` |
| **35%** | [Node (Default)](#node-default) | `26414` | `8923` | `60801` |
| **30%** | [Fastify](#fastify) | `22867` | `7115` | `35783` |
| **28%** | [Hono](#hono) | `20770` | `5869` | `29729` |
| **24%** | [Koa](#koa) | `18109` | `6818` | `57142` |
| **11%** | [Carbon](#carbon) | `8087` | `1391` | `10469` |
| **8%** | [Express](#express) | `6388` | `1004` | `8442` |


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
    Reqs/sec      8720.54    4725.29   59277.43
    Latency        5.73ms     4.56ms   384.65ms
    HTTP codes:
      1xx - 0, 2xx - 92857, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7143
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7143
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
    Reqs/sec      6463.63    1051.18    8473.25
    Latency        7.73ms     3.49ms   337.82ms
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
    Reqs/sec     23104.33    7124.20   35917.91
    Latency        2.16ms     1.99ms   182.62ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.24MB/s
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
    Reqs/sec     20456.03    5658.81   30193.75
    Latency        2.44ms     1.93ms   178.91ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.62MB/s
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
    Reqs/sec     63777.55    4132.85   78406.13
    Latency      781.86us    67.93us     3.28ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.06MB/s
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
    Reqs/sec     17856.78    5792.75   45501.32
    Latency        2.80ms     2.29ms   202.03ms
    HTTP codes:
      1xx - 0, 2xx - 95352, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4648
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4648
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
    Reqs/sec     25912.40    7799.21   40428.26
    Latency        1.93ms     1.85ms   165.40ms
    HTTP codes:
      1xx - 0, 2xx - 98380, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1620
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1620
    Throughput:     5.84MB/s
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
    Reqs/sec     73328.30    5506.94   79108.93
    Latency      679.95us   247.46us    11.57ms
    HTTP codes:
      1xx - 0, 2xx - 97190, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2810
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2810
    Throughput:    11.27MB/s
  ```


