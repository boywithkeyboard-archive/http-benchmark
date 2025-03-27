## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `74102` | `6008` | `85530` |
| **85%** | [Hyper Express](#hyper-express) | `62664` | `2732` | `70948` |
| **35%** | [Node (Default)](#node-default) | `26124` | `7396` | `55659` |
| **33%** | [Fastify](#fastify) | `24363` | `7278` | `36052` |
| **28%** | [Hono](#hono) | `20943` | `5734` | `30005` |
| **25%** | [Koa](#koa) | `18843` | `7887` | `62186` |
| **11%** | [Carbon](#carbon) | `8241` | `1418` | `10170` |
| **9%** | [Express](#express) | `6462` | `1076` | `8396` |


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
    Reqs/sec      9134.84    5360.90   57394.24
    Latency        5.47ms     4.39ms   379.85ms
    HTTP codes:
      1xx - 0, 2xx - 90707, 3xx - 0, 4xx - 0, 5xx - 0
      others - 9293
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 9293
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
    Reqs/sec      6888.84    3752.78   51698.27
    Latency        7.24ms     4.00ms   378.41ms
    HTTP codes:
      1xx - 0, 2xx - 93382, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6618
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6618
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
    Reqs/sec     24179.05    6854.16   36282.75
    Latency        2.07ms     1.97ms   177.22ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     5.49MB/s
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
    Reqs/sec     21224.88    5738.76   30054.20
    Latency        2.35ms     2.03ms   181.33ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.80MB/s
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
    Reqs/sec     62088.96    4973.57   91716.47
    Latency      808.27us   103.94us     4.13ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.76MB/s
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
    Reqs/sec     18684.78    6141.43   51004.09
    Latency        2.67ms     2.45ms   214.08ms
    HTTP codes:
      1xx - 0, 2xx - 94910, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5090
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5090
    Throughput:     4.01MB/s
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
    Reqs/sec     25268.96    7462.88   52536.11
    Latency        1.97ms     1.87ms   161.00ms
    HTTP codes:
      1xx - 0, 2xx - 97579, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2421
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2421
    Throughput:     5.65MB/s
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
    Reqs/sec     73658.43    5197.01   84665.25
    Latency      676.60us   175.38us     6.65ms
    HTTP codes:
      1xx - 0, 2xx - 97779, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2221
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2221
    Throughput:    11.40MB/s
  ```


