## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74343` | `5998` | `89498` |
| **86%** | [Hyper Express](#hyper-express) | `63725` | `4024` | `68708` |
| **35%** | [Node (Default)](#node-default) | `26028` | `8344` | `58993` |
| **31%** | [Fastify](#fastify) | `23170` | `6305` | `35902` |
| **29%** | [Hono](#hono) | `21284` | `6127` | `30343` |
| **24%** | [Koa](#koa) | `17655` | `6177` | `55925` |
| **11%** | [Carbon](#carbon) | `8143` | `1354` | `10423` |
| **9%** | [Express](#express) | `6563` | `1067` | `8470` |


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
    Reqs/sec      8652.38    3527.77   43816.74
    Latency        5.77ms     4.27ms   371.31ms
    HTTP codes:
      1xx - 0, 2xx - 95166, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4834
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4834
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
    Reqs/sec      6558.31    1061.12    8501.12
    Latency        7.62ms     3.58ms   340.60ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.88MB/s
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
    Reqs/sec     23318.29    6955.65   35328.63
    Latency        2.14ms     1.98ms   177.09ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.29MB/s
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
    Reqs/sec     20364.18    5380.31   29314.72
    Latency        2.45ms     1.87ms   170.87ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.60MB/s
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
    Reqs/sec     64574.02    4414.25   84323.82
    Latency      772.29us    78.39us     5.20ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.17MB/s
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
    Reqs/sec     18488.97    6404.99   55484.76
    Latency        2.70ms     2.27ms   197.87ms
    HTTP codes:
      1xx - 0, 2xx - 94430, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5570
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5570
    Throughput:     3.94MB/s
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
    Reqs/sec     25784.99    7970.21   47664.40
    Latency        1.93ms     1.91ms   165.03ms
    HTTP codes:
      1xx - 0, 2xx - 98175, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1825
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1825
    Throughput:     5.79MB/s
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
    Reqs/sec     74253.00    5895.18   80283.22
    Latency      671.02us   192.39us    12.52ms
    HTTP codes:
      1xx - 0, 2xx - 97289, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2711
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2711
    Throughput:    11.43MB/s
  ```


