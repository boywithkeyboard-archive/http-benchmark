## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75466` | `5258` | `80293` |
| **85%** | [Hyper Express](#hyper-express) | `64155` | `3969` | `67277` |
| **34%** | [Node (Default)](#node-default) | `25771` | `7468` | `46627` |
| **31%** | [Fastify](#fastify) | `23628` | `6606` | `35743` |
| **28%** | [Hono](#hono) | `20818` | `5605` | `29782` |
| **24%** | [Koa](#koa) | `18192` | `5798` | `44993` |
| **11%** | [Carbon](#carbon) | `8224` | `1358` | `10456` |
| **9%** | [Express](#express) | `6554` | `1077` | `8520` |


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
    Reqs/sec      8615.07    3337.91   41865.97
    Latency        5.79ms     4.24ms   368.41ms
    HTTP codes:
      1xx - 0, 2xx - 95463, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4537
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4537
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
    Reqs/sec      6599.39    1065.31    8541.17
    Latency        7.57ms     3.61ms   343.37ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.89MB/s
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
    Reqs/sec     24080.54    7128.21   35810.56
    Latency        2.08ms     2.01ms   181.29ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.46MB/s
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
    Reqs/sec     20632.19    5488.83   28939.18
    Latency        2.42ms     1.98ms   178.68ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.66MB/s
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
    Reqs/sec     63882.17    3782.05   68665.64
    Latency      779.34us    80.24us     4.34ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.09MB/s
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
    Reqs/sec     18328.03    6744.28   56771.91
    Latency        2.71ms     2.28ms   196.33ms
    HTTP codes:
      1xx - 0, 2xx - 93126, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6874
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6874
    Throughput:     3.87MB/s
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
    Reqs/sec     26041.40    7478.54   40011.09
    Latency        1.92ms     1.92ms   166.58ms
    HTTP codes:
      1xx - 0, 2xx - 98329, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1671
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1671
    Throughput:     5.85MB/s
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
    Reqs/sec     75683.39    5516.44   84094.56
    Latency      658.51us   199.85us     9.15ms
    HTTP codes:
      1xx - 0, 2xx - 97504, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2496
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2496
    Throughput:    11.67MB/s
  ```


