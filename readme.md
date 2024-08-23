## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `73869` | `5284` | `83374` |
| **87%** | [Hyper Express](#hyper-express) | `64595` | `3962` | `76370` |
| **35%** | [Node (Default)](#node-default) | `26056` | `7276` | `55008` |
| **32%** | [Fastify](#fastify) | `23558` | `7305` | `35988` |
| **29%** | [Hono](#hono) | `21259` | `6150` | `30838` |
| **24%** | [Koa](#koa) | `17532` | `6071` | `54086` |
| **11%** | [Carbon](#carbon) | `8365` | `1424` | `10480` |
| **9%** | [Express](#express) | `6520` | `1020` | `8485` |


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
    Reqs/sec      8627.99    4233.85   59465.20
    Latency        5.78ms     4.26ms   367.40ms
    HTTP codes:
      1xx - 0, 2xx - 93684, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6316
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6316
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
    Reqs/sec      6513.30    1022.34    8448.36
    Latency        7.67ms     3.55ms   341.36ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.86MB/s
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
    Reqs/sec     23307.67    7083.19   36115.82
    Latency        2.14ms     1.95ms   176.48ms
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
    Reqs/sec     20729.50    5617.32   29879.11
    Latency        2.41ms     1.97ms   178.17ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.68MB/s
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
    Reqs/sec     63803.12    4035.09   76654.65
    Latency      781.75us    79.90us     6.27ms
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
    Reqs/sec     17872.43    6351.08   59323.71
    Latency        2.79ms     2.35ms   205.64ms
    HTTP codes:
      1xx - 0, 2xx - 94647, 3xx - 0, 4xx - 0, 5xx - 0
      others - 5353
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 5353
    Throughput:     3.82MB/s
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
    Reqs/sec     27231.90    8554.67   58064.61
    Latency        1.83ms     1.84ms   157.55ms
    HTTP codes:
      1xx - 0, 2xx - 97336, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2664
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2664
    Throughput:     6.07MB/s
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
    Reqs/sec     73148.95    5888.29   80392.92
    Latency      680.33us   229.94us    13.48ms
    HTTP codes:
      1xx - 0, 2xx - 96631, 3xx - 0, 4xx - 0, 5xx - 0
      others - 3369
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 3369
    Throughput:    11.18MB/s
  ```


