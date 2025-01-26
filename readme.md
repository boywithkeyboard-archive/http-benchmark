## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75388` | `5659` | `90969` |
| **86%** | [Hyper Express](#hyper-express) | `64552` | `4164` | `72285` |
| **35%** | [Node (Default)](#node-default) | `26546` | `7395` | `53115` |
| **32%** | [Fastify](#fastify) | `24189` | `6988` | `36687` |
| **28%** | [Hono](#hono) | `21309` | `6232` | `30498` |
| **25%** | [Koa](#koa) | `18711` | `6670` | `60364` |
| **11%** | [Carbon](#carbon) | `8335` | `1442` | `10438` |
| **9%** | [Express](#express) | `6452` | `1042` | `8535` |


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
    Reqs/sec      8843.45    4628.74   58569.50
    Latency        5.64ms     4.28ms   375.25ms
    HTTP codes:
      1xx - 0, 2xx - 93808, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6192
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6192
    Throughput:     1.88MB/s
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
    Reqs/sec      7050.26    4302.97   49448.33
    Latency        7.08ms     3.86ms   352.37ms
    HTTP codes:
      1xx - 0, 2xx - 92512, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7488
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7488
    Throughput:     1.87MB/s
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
    Reqs/sec     24048.20    7118.23   36860.33
    Latency        2.08ms     2.03ms   184.01ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.45MB/s
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
    Reqs/sec     20901.11    5559.50   29531.59
    Latency        2.39ms     2.03ms   179.26ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.72MB/s
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
    Reqs/sec     62580.11    3713.71   67474.56
    Latency      796.75us    72.03us     3.15ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.89MB/s
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
    Reqs/sec     19266.81    7085.65   57841.68
    Latency        2.59ms     2.31ms   204.25ms
    HTTP codes:
      1xx - 0, 2xx - 93065, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6935
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6935
    Throughput:     4.05MB/s
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
    Reqs/sec     25374.17    7519.81   62894.75
    Latency        1.97ms     1.87ms   159.19ms
    HTTP codes:
      1xx - 0, 2xx - 97162, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2838
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2838
    Throughput:     5.64MB/s
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
    Reqs/sec     74933.04    4810.30   85961.24
    Latency      665.18us   169.64us     9.19ms
    HTTP codes:
      1xx - 0, 2xx - 97574, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2426
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2426
    Throughput:    11.57MB/s
  ```


