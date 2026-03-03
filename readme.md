## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `68666` | `3324` | `74570` |
| **84%** | [Hyper Express](#hyper-express) | `57443` | `3543` | `65328` |
| **31%** | [Fastify](#fastify) | `21579` | `6689` | `36007` |
| **30%** | [Node (Default)](#node-default) | `20773` | `5819` | `67454` |
| **30%** | [Hono](#hono) | `20418` | `6776` | `30114` |
| **26%** | [Koa](#koa) | `17536` | `7543` | `61610` |
| **11%** | [Carbon](#carbon) | `7723` | `1471` | `10074` |
| **9%** | [Express](#express) | `5940` | `1018` | `8035` |


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
    Reqs/sec      8604.53    5473.62   69259.11
    Latency        5.79ms     4.43ms   384.71ms
    HTTP codes:
      1xx - 0, 2xx - 91727, 3xx - 0, 4xx - 0, 5xx - 0
      others - 8273
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 8273
    Throughput:     1.79MB/s
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
    Reqs/sec      5908.31    1047.23    8204.31
    Latency        8.46ms     3.74ms   359.75ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.69MB/s
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
    Reqs/sec     21024.03    6203.49   35795.40
    Latency        2.38ms     2.08ms   186.16ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.77MB/s
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
    Reqs/sec     19647.76    6075.40   29730.09
    Latency        2.54ms     2.12ms   192.35ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.44MB/s
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
    Reqs/sec     57256.48    3692.04   68643.85
    Latency        0.87ms    88.50us     4.21ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.13MB/s
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
    Reqs/sec     17864.00    9975.45   76216.96
    Latency        2.79ms     2.51ms   221.92ms
    HTTP codes:
      1xx - 0, 2xx - 88705, 3xx - 0, 4xx - 0, 5xx - 0
      others - 11295
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 11295
    Throughput:     3.59MB/s
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
    Reqs/sec     20844.58    4801.98   59018.71
    Latency        2.39ms     2.00ms   175.47ms
    HTTP codes:
      1xx - 0, 2xx - 97589, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2411
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2411
    Throughput:     4.66MB/s
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
    Reqs/sec     68555.26    2992.82   77666.94
    Latency      726.77us   127.92us     6.76ms
    HTTP codes:
      1xx - 0, 2xx - 97454, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2546
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2546
    Throughput:    10.57MB/s
  ```


