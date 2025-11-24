## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74644` | `2762` | `81229` |
| **84%** | [Hyper Express](#hyper-express) | `62454` | `3400` | `64742` |
| **33%** | [Node (Default)](#node-default) | `24348` | `7886` | `76092` |
| **31%** | [Fastify](#fastify) | `23279` | `7545` | `36366` |
| **28%** | [Hono](#hono) | `20580` | `5897` | `29150` |
| **25%** | [Koa](#koa) | `18988` | `8035` | `66714` |
| **11%** | [Carbon](#carbon) | `8280` | `1422` | `10383` |
| **8%** | [Express](#express) | `6204` | `946` | `8334` |


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
    Reqs/sec      9019.89    6911.42   78250.05
    Latency        5.53ms     4.23ms   365.20ms
    HTTP codes:
      1xx - 0, 2xx - 89936, 3xx - 0, 4xx - 0, 5xx - 0
      others - 10064
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 10064
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
    Reqs/sec      6300.82    1009.28    8209.37
    Latency        7.93ms     3.56ms   343.19ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.80MB/s
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
    Reqs/sec     26145.37    8519.32   36281.14
    Latency        1.91ms     2.02ms   178.85ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.93MB/s
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
    Reqs/sec     20631.38    5818.59   28916.32
    Latency        2.42ms     2.07ms   186.76ms
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
    Reqs/sec     61987.01    3370.14   65289.04
    Latency      804.12us    69.44us     2.87ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.81MB/s
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
    Reqs/sec     18051.44    7684.72   66610.98
    Latency        2.76ms     2.34ms   206.84ms
    HTTP codes:
      1xx - 0, 2xx - 93202, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6798
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6798
    Throughput:     3.81MB/s
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
    Reqs/sec     26476.47   10788.77   81135.84
    Latency        1.88ms     1.96ms   169.40ms
    HTTP codes:
      1xx - 0, 2xx - 93722, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6278
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6278
    Throughput:     5.69MB/s
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
    Reqs/sec     75380.20    3129.22   84504.91
    Latency      659.94us   170.17us     9.10ms
    HTTP codes:
      1xx - 0, 2xx - 96431, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3569
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3569
    Throughput:    11.51MB/s
  ```


