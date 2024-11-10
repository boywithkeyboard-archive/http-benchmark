## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `75010` | `5948` | `82060` |
| **86%** | [Hyper Express](#hyper-express) | `64195` | `4517` | `67922` |
| **35%** | [Node (Default)](#node-default) | `26325` | `7392` | `57350` |
| **33%** | [Fastify](#fastify) | `24513` | `7135` | `36220` |
| **28%** | [Hono](#hono) | `21223` | `5769` | `30203` |
| **24%** | [Koa](#koa) | `18033` | `6567` | `56017` |
| **11%** | [Carbon](#carbon) | `8319` | `1353` | `10673` |
| **9%** | [Express](#express) | `6669` | `1064` | `8655` |


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
    Reqs/sec      8796.91    3511.27   45897.83
    Latency        5.67ms     4.15ms   371.88ms
    HTTP codes:
      1xx - 0, 2xx - 95313, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4687
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4687
    Throughput:     1.91MB/s
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
    Reqs/sec      6948.27    4188.05   56214.49
    Latency        7.16ms     3.72ms   337.26ms
    HTTP codes:
      1xx - 0, 2xx - 92114, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7886
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7866
      dial tcp 127.0.0.1:3000: connect: connection reset by peer - 20
    Throughput:     1.84MB/s
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
    Reqs/sec     23818.31    6331.99   36799.66
    Latency        2.10ms     1.97ms   176.88ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.40MB/s
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
    Reqs/sec     21887.60    5684.29   29693.41
    Latency        2.28ms     1.90ms   171.70ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.95MB/s
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
    Reqs/sec     63946.21    4276.24   68467.69
    Latency      778.23us    77.13us     4.57ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     9.10MB/s
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
    Reqs/sec     18351.61    5954.43   43007.23
    Latency        2.72ms     2.37ms   211.48ms
    HTTP codes:
      1xx - 0, 2xx - 95312, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4688
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4688
    Throughput:     3.95MB/s
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
    Reqs/sec     26752.41    7266.91   54039.04
    Latency        1.86ms     1.82ms   155.09ms
    HTTP codes:
      1xx - 0, 2xx - 97750, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2250
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2250
    Throughput:     5.99MB/s
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
    Reqs/sec     75553.44    6127.99   87660.53
    Latency      660.17us   235.42us    13.86ms
    HTTP codes:
      1xx - 0, 2xx - 98106, 3xx - 0, 4xx - 0, 5xx - 0
      others - 1894
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 1894
    Throughput:    11.72MB/s
  ```


