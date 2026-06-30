## http-benchmark

This repository compares the performance of some of the most popular web frameworks for Node.js against `node:http` using [bombardier](https://github.com/codesenberg/bombardier).

```bash
bombardier -n 100000 -c 50 -p r http://127.0.0.1:3000
```

### Summary

| RELATIVE | FRAMEWORK | AVG | STDDEV | MAX |
| :--- | :--- | :--- | :--- | :--- |
| **100%** | [uWS](#uws) | `69481` | `3773` | `81181` |
| **84%** | [Hyper Express](#hyper-express) | `58248` | `3473` | `69670` |
| **30%** | [Fastify](#fastify) | `20597` | `6023` | `35692` |
| **28%** | [Hono](#hono) | `19550` | `5601` | `30569` |
| **28%** | [Node (Default)](#node-default) | `19419` | `4879` | `61918` |
| **27%** | [Koa](#koa) | `18994` | `8774` | `65451` |
| **10%** | [Carbon](#carbon) | `7220` | `1208` | `10394` |
| **9%** | [Express](#express) | `6012` | `1081` | `8325` |


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
    Reqs/sec      7373.60    4370.72   56580.63
    Latency        6.76ms     4.65ms   401.15ms
    HTTP codes:
      1xx - 0, 2xx - 93639, 3xx - 0, 4xx - 0, 5xx - 0
      others - 6361
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 6361
    Throughput:     1.57MB/s
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
    Reqs/sec      5990.47    1060.36    8184.79
    Latency        8.34ms     4.05ms   389.35ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     1.71MB/s
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
    Reqs/sec     20488.80    5850.16   35575.75
    Latency        2.44ms     2.17ms   192.00ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     4.64MB/s
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
    Reqs/sec     21938.44    7042.49   30769.14
    Latency        2.28ms     2.12ms   186.71ms
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
    Reqs/sec     58453.45    3856.29   65358.00
    Latency        0.85ms   101.31us     3.30ms
    HTTP codes:
      1xx - 0, 2xx - 100000, 3xx - 0, 4xx - 0, 5xx - 0
      others - 0
    Throughput:     8.30MB/s
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
    Reqs/sec     18327.76    8180.92   62240.31
    Latency        2.72ms     2.54ms   218.05ms
    HTTP codes:
      1xx - 0, 2xx - 92554, 3xx - 0, 4xx - 0, 5xx - 0
      others - 7446
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 7446
    Throughput:     3.84MB/s
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
    Reqs/sec     19384.72    4904.27   56224.32
    Latency        2.57ms     2.00ms   172.94ms
    HTTP codes:
      1xx - 0, 2xx - 97550, 3xx - 0, 4xx - 0, 5xx - 0
      others - 2450
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 2450
    Throughput:     4.33MB/s
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
    Reqs/sec     69989.11    3861.24   79736.88
    Latency      710.77us   201.18us     8.94ms
    HTTP codes:
      1xx - 0, 2xx - 95180, 3xx - 0, 4xx - 0, 5xx - 0
      others - 4820
    Errors:
      dial tcp 127.0.0.1:3000: connect: connection refused - 4820
    Throughput:    10.53MB/s
  ```


